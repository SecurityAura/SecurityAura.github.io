---
title: 'Looking Back On #100DaysOfKQL'
description: 'Living your life, 1 query a day, for 100 days'
date: '2025-05-17T18:39:45.844Z'
categories: []
keywords: []
slug: /@securityaura/looking-back-on-100daysofkql-bf526b3d214e
---

**Living your life, 1 query a day, for 100 days**

### Premise

This blog post is about my #100DaysOfKQL challenge that I did from January 1, 2025, to April 12, 2025. For this challenge, every day I had to post a KQL query in my Github repo:

[**GitHub - SecurityAura/DE-TH-Aura: Repository where I hold random detection and threat hunting…**  
_Repository where I hold random detection and threat hunting queries that I come up with based on different sources of…_github.com](https://github.com/SecurityAura/DE-TH-Aura/tree/main "https://github.com/SecurityAura/DE-TH-Aura/tree/main")[](https://github.com/SecurityAura/DE-TH-Aura/tree/main)

That challenge was inspired by Matt Zorich (@reprise99 on Twitter/X) who did it a few years ago, but he actually went for 365 days. The #100DaysOfX challenge is quite popular, having people doing it this year for YARA as well.

In this blog post, I’ll be looking back on that challenge, how I lived it, what I learned and also, what future awaits my Github repo.

Alright, boring stuff out of the way, moving on!

### On a 1 to 10 scale, this was-

Holy was that kind of challenge hard. I have no idea how Matt (@reprise99 on Twitter/X) did it for 365 days straight, the absolute madman, but even just 100 days was enough to drain me of a lot of energy. And even there, it isn’t a real 100 days since I actually missed 2 days. For the first one, I had been up for 36 hours straight since my youngest was sick, and in the evening, I had a choice: put out a query, or go to bed. And I don’t regret my decision obviously, sleep is important. The second time could’ve been avoided to be honest. A new incident response mandate came in that day, I spent the whole evening on it and as I logged off to go to bed, noticed there was only 30 minutes left the day so … yeah. This is what happens when new IR mandates come in. The craze, the adrenaline, the excitation for the unknown. You forget about everything else.

The first 50–60 days or so I think went well. I’m generally happy with the queries I put out and what they can do. One thing you need to know about these queries is that either I had them already written up from previous experience, or I was writing them on the fly on the same day I was releasing them. And I just don’t write a query, post it and #YOLO hope it works. No, I test/reproduce the behavior I want to catch in my home lab to make SURE that the query works as intended. This is why putting these out wasn’t an adventure of a few minutes per day. It could take me up to 1 or 2 hours everyday to craft these because I had to do the testing and fine-tuning.

At some point though, you’ll notice that the quality of the queries, or at least, the page they came in, started falling short. This is where it started to get hard for me for various reasons, most of which were health-related (sickness, fatigue, etc.) most of which I’ll talk about in a bit. However, I do have plans for these, because I would not be OK with myself to leave to leave them in their current form.

### The obvious time constraint (x2)

For those of you who don’t know, I’m a dad of two young amazing kids, which means, I do not have as much free time as I used to have back then. At some point, when they grow older, I’ll have more free time. But right now, free time is basically 2–3h in the evenings during the week, 2–3h in the afternoon on the weekends (afternoon nap) and your usual 2–3h in the evenings. And at some point, the afternoon naps are going to stop which means I’ll only have my evenings. Another thing about me is that I’m a workaholic. Which means, most of my evenings I’ll spend working because, there’s just so much to do, and that I want to do. And I’m way more productive in the evenings than the day (“golden hours” per my girlfriend).

### An impression of déjà-vu

Those who have been paying attention to the queries may have noticed that sometimes, there will be a few queries centered around the same concept. May it be Windows Services, Scheduled Task, low prevalence binaries, etc. They may look similar to one another, but the reality is that, depending on where (which environment) you use them and what you are looking for, they may give entirely different results or even serve a different purpose. Let’s take Windows Services as an example. In total, I put out 4 queries for it:

Day 22 — Windows Service Creation or Modification With binpath via sc.exe.md  
Day 33 — Suspicious String in Service Creation ImagePath.md  
Day 44 — Windows Service Created Remotely.md  
Day 87 — Command Line Interpreter Launched as Service.md

_FYI: I just noticed the 22, 33, 44 suite while writing this blog post. It was not intended, but it made me chuckle._

Depending on your environment and how Windows Services are leveraged, the 4 of them could be used as detections or Threat Hunting queries. They are just 4 different ways to detect and/or hunt for suspicious/malicious behavior for Windows Services. You could literally, let’s say, come up with a Threat Hunting Playbook around Windows Services and each of those would be a valid query to run. And it doesn’t stop there. You could come up with queries such as:

Windows Service Created with a BinPath in an Usual Folder  
Windows Service Spawning a Low Prevalence Process  
Windows Service Spawning a Non-Signed or Invalidly Signed Process  
Etc.

All of these possible variants is what allows you to come up with an insane amount of queries that can be used as either detections or hunts within your environment. You just have to find out what works for your organization and put it in place.

### Building Blocks

A lot of the queries I put out are unfinished. By that I mean that I consider them to be base queries that can be built/improved upon. Why? Because depending on your organization, the base query may work as it is, or it may need further fine-tuning/refinement.

Let’s say the use of certain LOLBIN is strictly controlled in your environment through various means, having a query looking for these could be good enough to be turned into a detection right away, because you know that these should never, or very rarely, be used. On the other hand, if LOLBIN are used left and right in your environment that you cannot just hunt for their standard use, you’ll have to come up with a query (or multiple queries) that targets the very specific suspicious or malicious behavior you want to detect.

A base query is always good to get you started on a hunt, but if you want to turn it into a detection (if possible), it may need to be a developed a little bit more.

I even ended up rewriting some queries I had already written back then, because I had found ways to make them better in some fashion. Hence why you can see that some logic also comes back in multiple query. As with everything I create, I don’t consider the queries I put out to be in their final form. Just a form that worked for me at the time I put them out, and that I can always come back to in order to improve them at a different time.

### What’s next then?

You may be wondering what’s next with the GitHub repo following the end of the #100DaysOfKQL challenge. Worry not, I have a lot of plans for it that are just constrained by the free time I have available, as explained previously. Right now, there are three (4) things I want to do with the DE-TH-Aura repo:

1\. Go back to queries that were posted hastily and lack substance and complete them. May it be in terms of description, example, references, etc.  
2\. Review each queries to make sure that they have the appropriate level of information, in term of content. For instance, add the TTP table to each one of them like [@BertJanCyber](http://twitter.com/BertJanCyber "Twitter profile for @BertJanCyber") and many others do already.  
3\. Add additional queries for those that can be done across multiple products. For instance, if I have a query where I use DeviceLogonEvents, obviously it can also be done through the Windows Security Event Logs using SecurityEvent.  
4\. Create a repo architecture (folders) that will allow me to move/class queries where they belong.

Regarding #4, I’m basically talking about something alongside of let’s say, moving queries that uses DeviceProcessEvents to a Defender for Endpoint (MDE) folder. However, is that the only thing I want to do? Do I also want to further classify them into let’s say MITRE ATT&CK TTPs? However, how can I do that when a query hits multiple TTPs? Even more so: what do I do with query pages that have queries for Defender XDR products, but also SecurityEvent? SecurityEvent isn’t part of the Defender XDR suite, so should they still be attached to the page and be moved in a MDE folder? As you can see, I haven’t exactly worked out how I want to organize the repo and it’s something I can get stuck on for a long time. That’s just the kind of person I am. However, before I even get to #4, I still have to go through the first steps which means, I don’t have to make a decision right now.

### Obligatory shoutouts

I think it’s customary in these kind of posts to give some kind of shoutouts as well. At the very least, I want to highlight a few people here:

Matt (@reprise99 on Twitter/X) for inspiring me to do this challenge with his (you’re crazy, never doing it again)

Bert (@bertjancyber on Twitter/X) for having such an amazing repo and crafting a template that anyone can use for theirs

Jamie (@jamieanticosial on Twitter/X) , Andrew (@4ndr3w6s on Twitter/X) and Anton (@Antonlovesdnb) for being my #1 fans who ALWAYS liked my tweets first. At some point I suspected that Jamie was using some kind of macro or else to like my tweets not even 5 seconds after I pressed that Post button.

Nathan (@NathanMcNulty on Twitter/X ) and Dylan ([Dylan](https://medium.com/u/c35bb7c33160)) for replying to my tweets with the right Github URLs because I had changed the page name (and therefore, URL) after posting about it. Silly me, teehee~~~ (or say they say now I think).

And a lot more people in the background who reached out to me to say how they appreciate the challenge. Shoutout to these folks in Curated Intelligence (@CuratedIntel on Twitter/X) as well.

### The End!

If there’s anything I learned from this challenge is that there are just so many ways to write queries in KQL and there are so many things you can do with it if you just use the right filters, functions, etc. As someone who does not come from a programming background (I write PowerShell and I like it, that’s as much as I can do) I felt like KQL was easy enough to pick up and learn, on top of being flexible and powerful enough that it allows me to do what I need to do with it. I even ended up rewriting old queries I had created because while writing a new one, I came up with a better way of doing something and just wanted to apply that logic in every query where it was used.

Oh and obviously, never doing that kind of challenge again. At least, not until the kids moves out of the house which will be … in a while.

Till next time!

PS: [Ann Fam](https://medium.com/u/ded10e366d15) feel free to DM me any typos you find in this post, because considering how fast and unbothered I wrote this (there’s no DEV, only PROD), there’s bound to be a LOT!