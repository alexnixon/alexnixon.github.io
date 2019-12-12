--- 
layout: post 
title: "It's time to start writing" 
date: 2019-12-10 
--- 
 
I recently became aware of Jeff Bezos's policy of [banning PowerPoint](https://web.archive.org/web/20150730231457/https://blog.hirevue.com/sales/what-i-learned-from-jeff-bezos-about-sales-management) in meetings. The replacement is a multi-page "narrative" which is written by the meeting organizer (or team thereof) and distributed to all attendees at the start of the meeting - no pre-reading allowed - and discussion commences when everyone has made it to the end. 

This is the original email:

*From: Bezos, Jeff* \\
*Sent: Wednesday, June 09, 20014 6:02PM* \
*To: [REDACTED]*  
*Subject: Re: No powerpoint presentations from now on at steam*

*A little more to help with the question "why."* <br />
*Well structured, narrative text is what we're after rather than just text. If someone builds a list of bullet points in word, that would be just as bad as powerpoint.*
*The reason writing a good 4 page memo is harder than "writing" a 20 page powerpoint is because the narrative structure of a good memo forces better thought and better understanding of what's more important than what, and how things are related.*\
*Powerpoint-style presentations somehow give permission to gloss over ideas, flatten out any sense of relative importance, and ignore the interconnectedness of ideas.*\
\
*Jeff*

As someone writing their first blog post, it's interesting to note exactly what Jeff is focusing on here – it's the *creation* of the material rather the *consumption*. Even if the first item on the agenda of every meeting was a ceremony involving the printed pages of the narrative gliding towards the bin as a fleet of hastily folded planes, the policy will still have served its most important purpose. 

For a company trying to be "Earth's most customer-centric company", you might think that time would be best spent dealing directly with, say, *customers*, rather than writing text which will never be read. 
 
But Jeff isn't the only one who finds value in the process of writing. Some go further: 

<blockquote cite="David McCullough">Writing is thinking. To write well is to think clearly. That's why it's so hard.</blockquote>

And here I think it's worth digging further to ask - why is thinking clearly important? *Obviously* - and particularly in the context of software engineering -  if you don't think clearly then you won't think of as good solution as a competitor who does. 

That is undoubtedly true. But more importantly, writing a coherent and logical narrative forces your thoughts into a shape which can be easily communicated to others and used to persuade. None of us work in isolation. There is little point in having the answer if you cannot persuade others of its merit. 

<div class="centered">* * *</div> 
 
Somewhere in San Francisco there's a pizza surrounded by a team of engineers who, having been granted permission from above to migrate to the cloud, are vehemently disagreeing about how to architect their services. The Kubernetes guy knows that if only his solution were to be chosen then the future would be free from tedious discussions of solved problems like authentication and service discovery. Three slices to the right, the Nomad girl is brimming with confidence and espousing the virtues of single-purpose, orthogonal and composable components which are the key to principled software engineering. 
 
With the discussion dying down and a discarded crust the only food remaining, everyone has learned at least something new on some narrow technical aspect, but no one is persuaded and no consensus has emerged. Nowhere was a single narrative spun, capturing the entire journey from underlying business need to suggested implementation in a comprehensive, flowing and logical manner, instilling confidence that this is not just the best technology, it's the best choice for *our problem*, *our business*, and *our people* at *this point in time*.

Such narrative need not be a literary or aural masterpiece but it must be focused, comprehensive, accurate, unbiased, and it must flow.

In this case the contents could be:   

**The beginning** 

- Why was the decision made to move to the cloud? What were the underlying motivations? Do they pass the smell test? 
- What reason do we have for investigating k8s as a solution? What other alternatives are worth investigating too? 
- How high are the stakes? Are we laying the foundations of the cathedral or painting the bike shed? Is it likely we can easy switch later if things aren't working out? 
  
**The middle** 

- What's the mission statement of k8s? How does this tie in with our specific problem? 
- If we roll with it, what's the end goal - when migration is complete, what does the world look like? 
- How might we get from where we are to where we want to be with minimal disruption? What might the transition period look like? How long, roughly? 
- Are there any major unknowns or risks? Any existing in-house expertise? How do we have confidence there are none more outstanding? What steps might we take to reduce them? Play the devil's advocate and identify any and all flaws and address them head-on. 
- In what ways might this solution fare better than the alternative(s)? 
- In what ways might the alternative(s) fare better than this? 

**The end** 
- Relating back to the underlying business needs and the key points we've identified so far, why is k8s and our vision of how to get there the best? 

In this case, the technical content of the narrative will overlap substantially with a technical design document. But whereas a physical TDD itself is a meaningful output, the purpose of the narrative construction is the story left in the mind of the author. 

Whereas a TDD is specialised to matters of technical design. A narrative may provide clarity on any subject, from smaller issues of the choice of bug tracker up to the core vision, mission, and purpose of the company as a whole. 

For pure architectural questions a formal TDD may be produced as a complement but it is no substitute for a story. 

<div class="centered">* * *</div> 

I'm a little concerned that what was intended as a brief introduction has ended up as a rambling justification as to why *if no one reads this well I don't care anyway*. On behalf of my fragile ego I can only apologise.
