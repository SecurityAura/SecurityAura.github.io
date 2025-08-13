---
title: 'Threat Hunting vs Compromise Assessment: What’s the difference? (Opinion)'
description: Premise
date: ''
draft: true
categories: []
keywords: []
slug: ''
---

### Premise

Warning: Wall of text ahead! Sorry for the screenshots/images lovers (and I am one!).

The opinion presented in this blog post is my own and is therefore, just one amongst many that may exist on that subject. You may disagree with me and that’s fine. You may agree with me and that’s also fine. You may agree with some of the stuff I say, and disagree on the other. And that’s fine as well. An opinion is not a universal truth and should not be taken as such. I just hope that this blog will help people understand a bit more some concepts about Threat Hunting, Compromise Assessment and what is (or could be) the difference between the two.

I’ve been pondering for a while about posting this blog. Since it’s Friday night and I already did my dailies in Throne and Liberty, there’s nothing else for me to do then go to bed in an hour or so (aging will do that to you at some point). I figured now was a good time to get it out of my mind.

Also, for those who haven’t caught on yet, English is not my first language. So please excuse any typos, spelling or grammar mistakes that may find their way in this post. Feel free to DM me on Twitter (yeah, I still call it Twitter, @ me) if you spot one and I’ll try to fix it ASAP.

### Introduction

You’ve probably already heard the terms “Threat Hunting” and “Compromise Assessment” at some point in your InfoSec career. If you’ve just read these words for the first time now, the overall idea is this:

*   Threat Hunting: The proactive search of suspicious and/or malicious activity/behavior in an environment (e.g.: a network) in order to uncover threats that could have gone undetected by the security solutions in place.
*   Compromise Assessment: A high-level investigation of an environment where the goal is to try to determine if it is compromised by doing a deep dive and looking at everything and anything (with some form of methodology at play here, obviously).

If you go ahead and Google (or Bing, no one is judging you here) these concepts, you’ll find out that they pretty much amount to this. Though it may depend on which vendor’s website you end up on … anyway.

However, it seems like, at times, these terms are interchanged and used to describe the same thing. And this is why I wanted to write that blog post. To put on paper (no comments) what I think each concept means and how they are different.

### Threat Hunting

Depending on who you ask, Threat Hunting can take many forms. Some people will say that investigating an alert at scale is threat hunting, others will say it’s just a normal alert investigation. Others will say that looking on the network for IOCs extracted from an on-going forensics investigation is threat hunting. And so on, and so on. I vaguely remember putting a poll up on Twitter at some point about this but, I can’t be bothered to find out. If you do, feel free to DM me and I’ll add it to the post.

If I were to define Threat Hunting, and that’s exactly what I want to do in this post, it would go a bit like this:

*   The proactive search of suspicious and/or malicious activity/behavior in an environment (e.g.: a network) in order to uncover threats that could have gone undetected by the security solutions in place (look at me trying to increase the word count by copy/pasting stuff I’ve already wrote)
*   Threat Hunting takes place on data (e.g.: telemetry) that has already been collected and therefore, does not need to be seeked out (e.g.: download a log file, Event Log, etc.)
*   Threat Hunting takes place in centralized consoles and/or solutions that allows you to query this data and get results back, allowing you to analyze it directly and/or externally (e.g.: export to results to CSV, do some magical Python or PowerShell parsing on it, blabla…)
*   You’re looking for a specific, very defined thing. May it be an IOC, IOA, suspicious and/or malicious behavior, etc. You don’t just go: I’m going to do a threat hunt for users that could have been phishing. Well, you can, but your hunt can’t possibly cover all the scenarios that exists. You’ll have to drill down on some of them. Which goes back to hunting on something that is specific/defined.
*   And the most important, is a CONTINUOUS activity which means, it is NOT a one-time and done kind of deal. Unless you managed to turn your hunts into detection opportunities. And I love when this happens.

With this in mind, we can summarize Threat Hunting to being something that is:

*   Proactive
*   Leverage existing data
*   Is done in a “central” fashion
*   Look for specific/defined things and is continuous.

### Compromise Assessment

Now Compromise Assessment on the other hand is a bit more broad. The clue is in the pudding (I have no idea if I’m even using that expression right, but at 9:30 PM on a Friday night, I find it funny to put it in this blog post): you’re trying to see if an environment has been compromised. Which means, you have to look at more than multiple specific things. You have to go large, cast wide nets and see what bites. In my opinion a Compromise Assessment:

*   Takes place on data that may not have been collected yet. Which means, it has to be sourced. For instance, grabbing logfiles from a network appliance through a support package/bundle or grabbing the Windows Event Log from a Windows Server
*   Can take place on a whole environment or in a more defined scope, for instance, network appliances at the edge of a network.
*   Is reactive to some sort of information and/or data that was uncovered and/or which makes you worry that an attacker could be in the network. Like a new shiny 0-day that gets dropped against your multi-million dollars next-gen unpatched (and outdated) VPN appliance (if reading this hits too close to home well … I feel you).

In other words, when you do a Compromise Assessment, it can be one of two things: you either don’t know what you’re looking for, you’re just looking for overall suspicious/malicious activity which would indicate a compromise, or you have a general idea of what you’re looking for