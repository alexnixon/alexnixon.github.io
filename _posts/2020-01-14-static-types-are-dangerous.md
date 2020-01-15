---
layout: post
title: "Static types are dangerous"
teaser: "You can use static types to forbid certain bad behaviours from happening at runtime. However, they don't come for free. This post walks through a real-world example in Haskell and highlights a danger that often arises."
image: /img/dynamic-types-funny-sign.png
published: true
---

Static type systems allow you to forbid certain bad behaviours from happening at runtime.

That's a good thing.

However, I consistently underestimate just how difficult they can be to wield, even with cases as simple as the mismatched units in the funny signpost above.

This post will take you through a tiny, real-world example, showing how you can use static types to swap one problem for another.

There will be code. Not much, but some. It's in Haskell though, so apologies to 95% of you. And it's in my ugly beginner's Haskell, so apologies to the other 5% too.

## The context

I work in finance. That means I go to the office, take off my coat, unlock my PC, then twist the legs off puppies [^1] until my hands cramp. I then use what time in the day remains to write programs that handle money.

[^1]: LEGAL DISCLAIMER: that is obviously a joke. Twisting would give me callouses. But also I quite like puppies - [here's a picture](/img/bella.jpg) of Bella, my family's dog.

Within those programs I *really* don't want to accidentally add $3 to £4. If you logged on to your internet banking and saw a balance of £$7, you'd probably throw your laptop out of the window and Forrest Gump your way to a cave in the mountains, never to return.

I would hazard a guess that most - if not all - Haskellites would tell you that with static types, preventing these kinds of errors is a walk in the park. And on that walk in the park you should avoid eye contact with any disheveled python programmers you come across, sitting cross-legged in puddles eating clods of mud with one hand and counting their dollarypounds with the other[^python-legal].

[^python-legal]: LEGAL DISCLAIMER 2: that is obviously a joke. Python is great, Pythonistas are great, and optional typing exists.


Anyway - I tried to use Haskell to write some pure functional code which prevents amounts of different currencies from being added together. It sounds easy but I couldn't do it.

This is my first attempt at writing Haskell so the problem is quite likely that I suck. But I'm not ruling out the possibility that I suck *and also* this is far more difficult than it first seems.

Let's begin.

## The problem

I'd like model a list of trades. The data looks like this:

    Quantity, Ticker, Price, Currency
    100,      VOD.L,  1,     GBP
    200,      VOD.L,  2,     GBP
    300,      AAPL.O, 3,     USD
    50,       4151.T, 5,     JPY

The first line here means: a trade of **100** shares of **VOD.L** were bought at a price of **1** **GBP**[^ccy].

[^ccy]: A GBP is a Great British Pound (£). USD is United States Dollars (£), and JPY is Japanese Yen (¥)

Then, I want to write a function which calculates the *total notional in each currency*. The word *notional* is a fancy way of saying `price * quantity`. Think of it as "value of the thing that changed hands".

For illustration, the function signature might look something like this:

```hs
sumNotionals :: [Trade] -> Map Currency Rational
```

In English, it's a function that takes a list of trades and returns a map from currency to quantity.

In this example the output would be:

    Currency, Notional
    GBP,      500
    USD,      900
    JPY,      250

Where `GBP` notional was calculated as `100*1 + 200*2 = 500`, `USD` is `300*3 = 900`, and `JPY` is `50*5 = 250`

Nothing too complicated there, so let's model our data. Recall a line of our input:

```
Quantity, Price, Currency, Ticker
100,      1,     GBP,      VOD.L
```

All fields aside from price are quite straightforward:

```hs
type Quantity = Int

data Currency
  = USD
  | GBP
  | JPY
  deriving (Eq, Ord, Show)

newtype Ticker = Ticker String
```

The only thing left is the price: money. I'm going to implement a money type, along with addition, multiplication, and the `sumNotionals` function in three different ways.

Each time I'll use stronger typing than the last in an effort to reduce the space of possible runtime errors and ascend to typing nirvana.

### Attempt 1 - partially correct

My first attempt is what I imagine the majority of (non-Haskell) programmers would come up with - each `Money` instance carries around with it a `Currency`, and when you try add two `Money`s together there is a runtime check that they have the same currency, and if there's a mismatch an error is thrown. In Haskell it looks like this:

```hs
data Money = Money
    { mQty :: Rational
    , mCcy :: Currency
    }

plus :: Money -> Money -> Money
plus (Money qty1 ccy1) (Money qty2 ccy2)
  | ccy1 == ccy2 = Money (qty1 + qty2) ccy1
  | ccy2 /= ccy2 = error $ "Currency mismatch"

mul :: Money -> Rational -> Money
mul m1 v = Money (mQty m1 * v) (mCcy m1)

-- This will compile and run fine:
x = Money 1 GBP `plus` Money 2 GBP == Money 3 GBP

-- This will compile but will throw an error when evaluated
x = Money 1 GBP `plus` Money 2 USD
```

That looks pretty good - the code won't produce incorrect values. And it's easy to model our trades with this too:

```hs
data Trade = Trade
    { tQty    :: Rational
    , tPrice  :: Money
    , tTicker :: Ticker
    }
```

And only slightly harder to write our final function, `sumNotionals`:

```hs
import qualified Data.Map.Strict as M

sumNotionals :: [Trade] -> M.Map Currency Money
sumNotionals trades =
  let notional trade = tPrice trade `mul` tQty trade
      notionals = fmap notional trades
      currencies = fmap mCcy notionals
  in M.fromListWith plus $ zip currencies notionals
```

This is very concise, and if you're comfortable reading Haskell, quite legible. It maps the list of trades to their individual notionals, zips that list with the currencies, then builds that into a map with the currency as the key, with any duplicate values are combined with our `plus` function.

This is all well and good aside from one problem: we've used a `plus` function which might throw an exception. That's pretty poor - we set out to write a program without errors but have written one which might explode.

So can we do better? Lets see.

### Attempt 2 - either works for me

The problem with our implementation so far is that the return type of the `plus` function is a lie. It says the function will return a `Money`, but we know that in the currency-mismatch case there might not be a `Money` that can be returned. So we can improve the return type by explicitly modelling this:

```hs
data Mismatch =
  Mismatch Currency Currency

type Result = Either Mismatch Money
```

Now we can write a `plus` function which will never fail at runtime:

```hs
plus :: Money -> Money -> Result
plus (Money qty1 ccy1) (Money qty2 ccy2)
  | ccy1 == ccy2 = Right $ Money (qty1 + qty2) ccy1
  | ccy1 /= ccy2 = Left $ Mismatch ccy1 ccy2
```

That's great, right? Well, not quite, because when we try implement `sumNotionals` we end up with a bit of a mess:

```hs
eitherPlus :: Result -> Result -> Result
eitherPlus (Right m1) (Right m2) = plus m1 m2
eitherPlus (Left e) _            = Left e
eitherPlus _ (Left e)            = Left e

sumNotionals :: [Trade] -> Either Mismatch (M.Map Currency Money)
sumNotionals trades =
  let notional trade = Right $ tPrice trade `mul` tQty trade
      notionals = fmap notional trades
      currencies = fmap (mCcy . tPrice) trades
  in sequence $ M.fromListWith eitherPlus $ zip currencies notionals
```

That's pretty ugly so let's figure out why.

Our `plus :: Money -> Money -> Result` function wasn't easy to compose because its output type is different to its input type. So we needed to define our own function [^3] - `eitherPlus` - which uses our `Result` in both the input and the output.

[^3]: this looks suspiciously like a semigroup concat - and it is! Unfortunately for us the default semigroup for `Either` just takes the left-hand value, rather than combining them as we'd like.

With that done, the implementation is quite similar to the previous one, with the main difference being the addition of a call to `sequence` which hoists the errors out of the map.

But something quite subtle has gone wrong. Look closely at the function signature:
```hs
[Trade] -> Either Mismatch (M.Map Currency Money)
```

We are forcing the caller to handle a currency mismatch even though **there is no possible combination of inputs to our function which can cause it to happen**.

This is *really* interesting: with our Attempt 1, `plus` was lying but `sumNotionals` was telling the truth. Now, `plus` is telling the truth but `sumNotionals` is lying.

When wrote `plus` the compiler was our friend, helpfully making sure we write code to handle all inputs. But as soon as we slightly scaled up our problem to `sumNotionals`, it turned against us. It isn't smart enough to realise that grouping by currency means all the values under a given key have the same currency, and so dogmatically insists we write code which will never be executed.

Tradeoffs in action!

But let's continue. Perhaps we can require that anyone calling our `plus` function first *proves* that the two currencies match. That way these ugly `Either`s should disappear entirely and we're home free.


### Attempt 3 - kindness begets kindness

As we just saw, the aim now is to prove to the compiler than any time we add two `Money`'s together, they definitely have the same currency. We'll need to somehow bring the currency into the type - perhaps something which looks like `MoneyGBP` and `MoneyUSD`. Luckily there's some prior art to draw on here - the [safe-money](https://hackage.haskell.org/package/safe-money) library. This approach relies on some extensions:

```hs
{-# LANGUAGE DataKinds      #-}
{-# LANGUAGE KindSignatures #-}
```

If you're unfamiliar with what we mean by "kind", it's just the type of a type. If you want to learn more then read [this](https://diogocastro.com/blog/2018/10/17/haskells-kind-system-a-primer/) excellent blog post.

Anyway - know you know all about kinds, we can define a `Money` type like so:

```hs
newtype Money (currency :: Currency) =
  Money { unMoney :: Rational }
```

The kind syntax is a bit unfamiliar, but all it is saying is that the `Money` type needs an argument (a type which is a `Currency`) before it can be used. In other words, its kind is `Currency -> *`.

This `Money` type can be used like so:

```hs
-- This compiles!
x = Money 42 :: Money GBP

-- This doesn't compile, because Int isn't a Currency
y = Money 42 :: Money Int
```

Easy! And the definition of type-safe `plus` and `multiply` is exactly as you'd expect:

```hs
plus :: Money ccy -> Money ccy -> Money ccy
plus m1 m2 = Money (unMoney m1 + unMoney m2)

multiply :: Money ccy -> Rational -> Money ccy
multiply m1 v = Money (unMoney m1 * v)
```

And we finally have the compile-time check we wanted!

```hs
gbp = Money 123 :: Money GBP
usd = Money 345 :: Money USD

thisCompiles = gbp `plus` gbp

thisDoesNotCompile = gbp `plus` usd

--     * Couldn't match type 'USD with 'GBP
--      Expected type: Money 'GBP
--        Actual type: Money 'USD
--    * In the second argument of `plus', namely `usd'
--      In the expression: gbp `plus` usd
--      In an equation for `thisDoesNotCompile': thisDoesNotCompile = gbp `plus` usd
```

This is all fine and dandy...until we try define our `Trade` data type again, using this new `Money` definition. We realise that we need to pull the currency up into the type signature of the trade too:

```hs
data Trade (ccy :: Currency) =
  Trade
    { tQty    :: Rational
    , tPrice  :: Money ccy
    , tTicker :: Ticker
    }
```

Anywhere in our program that we have a trade, we need to statically prove its currency. How can you possibly do that, when you might be reading unknown data from the outside world? Well, we could make a new type for that case:

```hs
data UnknownTrade
  = UsdTrade (Trade USD)
  | GbpTrade (Trade GBP)
  | JpyTrade (Trade JPY)
```

This seems reasonable, as we now have a way to represent trades where the currency is unknown. But if we then want to *do* anything with it, we'll need to pattern-match into each individual trade+currency type. Let's put this aside for now.

Anyway, let's implement our `sumNotionals` function one last time.

The first thing to do is figure out the type signature. The input side is easy - it should take a `[UnknownTrade]`. But the output side is more difficult - we need a "map where the keys are `Currency`s and the values are `Money ccy`s, where `ccy` is the same currency as is in the key". This is an example of [dependent types](https://en.wikipedia.org/wiki/Dependent_type), which aren't well supported in Haskell.

Unfortunately, this is where my Haskell skills fall short. If I were to guess the appropriate syntax, it would be something like this:

```hs
sumNotionals :: [UnknownTrade] -> Map (ccy :: Currency) (Money ccy)
```

That isn't valid Haskell, though I'd be curious as to whether the [dependent-map](https://hackage.haskell.org/package/dependent-map) package could be used to build a substitute. My guess would be that it could, but with my writing-production-code hat on I can feel myself beginning to tremor at the prospect of introducing something this advanced when other reasonable alternatives exist.

And returning to our `UnknownTrade` type - it has as many cases as the number of currencies we're modelling. In the real world there are a couple of hundred, and pattern matches that long are obviously unpalateable.

Is there a way of abstracting over this? I don't know, but am secretely hoping someone will tell me!

## Wrapping up

Well for a start we didn't make it to nirvana. But it's the journey that counts, right?

We started off implementing `Money` naively with a partial function. The implementation was concise and easy to read but would allow us to write code which could break at runtime.

We then made the error explicit in the return type, making the function total - it can no longer break as it forces the caller to handle the possibility that bad data was passed in. However, this lead us to forcing the caller into handling errors which can *never* happen.

Finally, we dipped our toes into a more advanced corner of the type system by using data kinds. It's incredibly neat when dealing with hard-coded data, but as soon as you pull in data from the outside world and process it in interesting ways, the type-level proofs quickly become cumbersome.

So - if you came across this problem in production, which approach would you take? Personally, I think I would use the `Money` definition from #1 and #2, and implement *both* of their addition functions:

```hs
data Money =
  Money
    { mQty :: Rational
    , mCcy :: Currency
    }

unsafePlus :: Money -> Money -> Money
unsafePlus (Money qty1 ccy1) (Money qty2 ccy2)
  | ccy1 == ccy2 = Money (qty1 + qty2) ccy1
  | ccy2 /= ccy2 = error $ "Currency mismatch"

plus :: Money -> Money -> Either Mismatch Money
plus (Money qty1 ccy1) (Money qty2 ccy2)
  | ccy1 == ccy2 = Right $ Money (qty1 + qty2) ccy1
  | ccy1 /= ccy2 = Left $ Mismatch ccy1 ccy2
```

Then the caller can choose which method to use. Are they absolutely certain the currencies match, like in our `sumNotionals` example? Use `unsafePlus`. Is there some doubt? Use `plus`.

The general theme here - which you'll recognise if you've used static typing yourself - is that choosing how to model your problem is *hard*. There are lots[^brice] of sensible ways of doing it and they each have their own tradeoffs.

And here we come onto the danger that static types pose to practical software engineering.

The challenge of finding the perfect types isn't just hard, it's also *really interesting*. This makes it all too easy to get sucked in to solving the meta-problem of writing beautiful, type-safe code, losing sight of the business problem that is, ultimately, all that matters.

We should all learn about purity, exceptions, monads, data kinds and dependent types - they will make us better programmers.

But don't forget - we're here to build systems, not to write proofs.

[^brice]:
    Hat tip to [Daniel Brice](https://twitter.com/fried_brice) for suggesting another approach - defining operations over a "bag of monies in different currencies":

    ```hs
    newtype Monies = Monies { unMonies :: M.Map Currency Rational }

    plusMonies :: Monies -> Monies -> Monies
    plusMonies (Monies m1) (Monies m2) = Monies $ M.unionWith (+) m1 m2
    ```

    It's really neat - by using a heterogenous-bag-of-money as your primary representation you can implement addition without worrying about currencies mismatching.

    However - unlike the data-kind approach, it does not allow you to take advantage of times when you *do* statically know currencies and want to take advantage of that fact, as was shown in the GBP-USD-failing-to-compile example.