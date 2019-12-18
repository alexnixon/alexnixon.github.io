--- 
layout: post 
title: "It's time to start writing" 
date: 2019-12-10 
--- 
 
I recently became aware of Jeff Bezos's policy of [banning PowerPoint](https://web.archive.org/web/20150730231457/https://blog.hirevue.com/sales/what-i-learned-from-jeff-bezos-about-sales-management) in meetings. The replacement is a multi-page "narrative" which is written by the meeting organizer (or team thereof) and distributed to all attendees at the start of the meeting - no pre-reading allowed - and discussion commences when everyone has made it to the end. 

This is the original email:

<div class="emailquote">
From: Bezos, Jeff <br />
Sent: Wednesday, June 09, 20014 6:02PM <br />
To: [REDACTED] <br />
Subject: Re: No powerpoint presentations from now on at steam <br />
<br />
A little more to help with the question "why." <br />
Well structured, narrative text is what we're after rather than just text. If someone builds a list of bullet points in word, that would be just as bad as powerpoint. <br />
The reason writing a good 4 page memo is harder than "writing" a 20 page powerpoint is because the narrative structure of a good memo forces better thought and better understanding of what's more important than what, and how things are related. <br />
Powerpoint-style presentations somehow give permission to gloss over ideas, flatten out any sense of relative importance, and ignore the interconnectedness of ideas. <br />
<br />
Jeff
</div>

From the biased position of someone writing their first blog post, I'm going to zoom in one a narrow point I believe Jeff is making here – it's primarily the *creation* of the material which forces better thought, not the *consumption*. Even if the first minute of every meeting involved the printed memo being ceremonially folded into a paper plane and propelled toward the bin, the act of writing would not be undone.

For a company trying to be [Earth's most customer-centric company](https://www.amazon.jobs/en/working/working-amazon), you might think that time would be best spent interacting with, say, *customers*, rather than crafting [multi-page narratives](https://www.sec.gov/Archives/edgar/data/1018724/000119312518121161/d456916dex991.htm). You could be forgiven for seeing a company with too much time on its hands, filling it with intellectual decadence.
 
But Amazon is far from the first to recognise the value in the writing process. Some go even further:

<blockquote cite="David McCullough">Writing is thinking. To write well is to think clearly. That's why it's so hard.</blockquote>

And here I think it's worth digging further to ask - why is thinking clearly important? *Obviously* - in our context of software engineering - if you don't think clearly then any solutions you craft as an individual will be suboptimal. 

That is undoubtedly true. But more importantly, writing a coherent and logical narrative forces your thoughts into a shape which can be easily communicated to others and used to persuade. None of us work in isolation. There is little point in having the answer if you cannot convince others of its merit.

<div class="centered">* * *</div> 
 
Somewhere in San Francisco there's a pizza encircled by a team of engineers who, having just heard of a new firm-wide mandate to migrate aging services to the cloud, are vehemently disagreeing about which technology to use. The Kubernetes guy knows that only his solution leads to a future free from tedious discussions of solved problems like authentication and service discovery. Three slices to the right the Nomad girl brims with confidence and espouses the virtues of single-purpose, orthogonal and composable components which are the *bedrock* of well-engineered software systems.

After half an hour of the discussion bouncing from feature to feature to pro to con to bug to feature, and with only discarded crust remaining, everyone has learned something new. But no one is persuaded and no consensus has emerged. Though a useful exercise, such unstructured discussions are not an efficient mechanism for reaching agreement.

What is needed is for a single individual[^1] to spin a narrative which captures the entire journey from underlying business needs through to practicalities of implementation, in a comprehensive, flowing and logical manner. This is necessary to instill the confidence in others that this is not just the best technology in an abstract sense, it's the right choice for *our problem*, *our business*, and *our people* at *this point in time*.

[^1]: more precisely I'd say *at least one* individual. If multiple people had conflicting initial thoughts then I would advocate they each produce their own narrative then bring them together with an [adversarial](https://en.wikipedia.org/wiki/Adversarial_process) approach, though that may not be suitable for all teams.

Such narrative need not be a literary masterpiece but it must be focused, comprehensive, accurate and unbiased, and it must flow.

For this particular situation - the choice of a technology - the contents would likely touch on:   

**The beginning** 

- What were the underlying reasons for the decision to change technologies? Why is this worthwhile embarking on and what benefits will be seen?
- What reason do we have for investigating this particular tech as a solution? What other alternatives are worth investigating? 
- How high are the stakes? Are we laying the foundations of the cathedral or painting the bike shed? Is it likely we can easy switch later if things aren't working out? 
  
**The middle** 

- Does the tech being suggested have a mission statement or core guiding principles? How does that tie in with our problem and philosophy? 
- If we roll with it, what's the end goal? When implementation is complete, what does the world look like? 
- How might we get from where we are to where we want to be with minimal disruption? What might the transition period look like? How long, roughly? 
- Are there any major unknowns or risks? Any existing in-house expertise? How do we have confidence there are none more outstanding? What steps might we take to reduce them? Play the devil's advocate and identify any and all flaws and address them head-on. 
- In what ways might this solution fare better than the alternative(s)? 
- In what ways might the alternative(s) fare better than this? 

**The end** 
- Relating back to the underlying business needs and all key points we've identified so far, why is this the best option? 

In this case, the technical content of the narrative would overlap substantially with a technical design document. But whereas a TDD is an end in itself, the physical output of this writing is merely a pleasant side-effect. The real output is the story left in the mind of the author. The process of organising thoughts into flowing natural language results in a deep understanding that translates into an ability to adapt the story at a moments notice to a level appropriate for convincing anyone of its merit, regardless of their level of technical expertise or involvement in discussions so far.

And where a TDD is specialised tool, narratives are general. They scale. From the narrowest of technical issues up to the most fundamental questions of the vision and purpose of the company as a whole, there are no problems which resist clear-thinking.

<div class="centered">* * *</div> 

On reflection I find it slighly amusing that what I intended on being a brief introductory blog post has ended up as a slightly rambling justification as to why *if no one reads anything I write well it doesn't matter anyway*. I feel like my fragile ego owes me an apology for taking up so much screen space.
