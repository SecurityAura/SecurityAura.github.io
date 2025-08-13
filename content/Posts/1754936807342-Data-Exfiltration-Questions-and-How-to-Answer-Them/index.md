---
title: 'Data Exfiltration: Questions and How to Answer Them'
description: >-
  An Incident Responder insights and stories on how to approach data
  exfiltration
date: '2024-09-15T18:22:40.170Z'
categories: []
keywords: []
slug: /@securityaura/data-exfiltration-questions-and-how-to-answer-them-84856b14003c
---

### Premise

Warning: Wall of text ahead! Sorry for the screenshots/images lovers (and I am one!).

The information presented in this article, on top of the various insights offered comes from my few years of experience in Incident Response. It is not an absolute truth and there are many different ways to answer the questions presented here. However, this article aims to give insight in what kind of questions are asked in incidents where data exfiltration occured or may have occurred and ways to answer them.

This article is mainly based on ransomware and/or data exfiltration incidents at a host and/or network-based level.

At some point in time, I may post other articles related to data exfil with additional insights and/or stories.

### Introduction

Whenever you deal with an incident where a threat actor managed to get access to an environment, one of the biggest investigation track you’ll have to work on is the data exfil one. In ransomware incidents and/or incidents where a victim received a ransom demand, the first and biggest question that will be asked is: did data exfiltration occur? In these incidents, if you can quickly identify which threat actor you’re dealing with, you can already assess with a good level of confidence if data was exfiltrated or not. That’s why, at the beginning of these incidents, one of the first steps will be to call in a breach coach. From an incident responder’s perspective, the breach coach is seen as the party that will assist the victim with assessing the risk of a data exfil and the data that would’ve been exfiltrated. They’ll also provide guidance on the notification process, should people whose data have been “stolen” need to be contacted.

While a lot of companies (even SMBs) may think that they do not hold any kind of sensitive data, such as PII, PHI, etc. and therefore, notification to impacted people would not be necessary, I typically ask them if they have any kind of HR and/or payroll information. Most of them will say yes, meaning that in these situations, these people (employees, ex-employees, contractors, etc.) would need to be notified. It shows that even in 2024, companies may not even be aware of the kind of data they hold, nor what they represent and their classification.

### The Questions

When it comes to data exfil, multiple questions need to be answered or in short: anything and everything. For the purpose of this article, you can see these questions as:

*   If — Did data exfil even occur? This is usually confirmed (or infirmed) by answering any of the remaining questions
*   How much — How much data was exfiltrated? Are we talking about a couple of documents? A few MB? Or multiple GB, even a whole TB?
*   How — How was the data exfiltrated? Was it uploaded to a cloud service? Was it sent via SFTP to a remote server in another country?
*   When — At which point in the incident did the data exfil occur? How long did it last? In ransomware cases, was it just before the ransomware payload was deployed? Was it days before?
*   What — The hardest question of all the answer: what data was exfiltrated? Which files, folders, network shares, etc. were exfiltrated by the threat actor during that attack?

The ability to answer these questions depends on so many factors (even a bit of luck sometimes) that it is impossible in my opinion and experience to go in a new incident and tell a victim that you’ll be able to answer all of them. You’ll always do your best at answering as many of them as you can in the most complete way possible too.

### If — Did data exfil even occur?

Depending on the incident and what kind of information is available to you when you get called in, you may already have the answer to that question. Or at very least, can heavily suspect the answer.

For instance, if you get called in on a ransomware where you can quickly identify the threat actor group (through the ransom note, encrypted file extension, etc.) and you know that this group is known to leak/publish stolen data (e.g.: a Dedicated/Data Leak Site (DLS)), you already know that chances of data exfil are high.

Otherwise, answering this question in a positive or negative fashion is usually dependent on the other questions. If you are able to positively answer any of the other questions, you pretty much confirmed that data exfil occurred.

### How much — How much data was exfiltrated?

Alongside the “What”, one of the hardest question to answer. Answering this question is typically highly dependent on having a way to measure/calculate the amount of data that went out of a network. May it be through firewalls, NDRs, the ISP and/or even host-based artifacts (such as SRU).

Most next-gen firewalls now have very nice dashboards that can help you quickly see how many GB of data was downloaded/uploaded in various situations: top source, top destination, top application, top web filter, etc. This is probably one of the easiest way to answer this question. For instance, if you see that the Top Source in the firewall dashboard is the file server that uploaded 100 GB of data, and when you drill down on the details, you see that 90 GB of that data was uploaded through port 22 to an IP address in another country that has no association with the victim, you can already deduce what you should do next. Investigate that server for artifacts which would show that data was explicitly sent out by the threat actor (or at least, through means or actions not known to the victim).

For firewalls and/or NDRs that do not have dashboards and/or reports available, you can usually still answer that question using the raw logs. If you can export the capture traffic logs, and these logs have the sent/received bytes fields, and you know how to interpret them (e.g.: are the values incremental), you can whip up a quick script in your favorite language and/or throw the logs in your favorite SIEM to calculate the amount of bytes that was sent out. Therefore, giving you the possible amount of data that was exfiltrated.

And in situations where the victim (or you, the Incident Responder, depending on how you look at it) does not have firewall logs available (e.g.: logs were in memory and it was rebooted before you get called in) or a NDR, from a network perspective, your last hope would be to request the logs from the ISP. If you’re lucky, the victim’s ISP will be able to generate bandwidth usage and/or download/upload reports for the last X days (covering the attack period). From there, if you’re able to answer the “When”, you may be able to get a glimpse as to how much data could have been exfiltrated, at a maximum. For instance, if you find out that data exfil occurred on the 14th of the month, and the ISP report shows that on that day, 150 GB of data went out, you can say that at the very least, a maximum of 150 GB of data (of an unknown type: raw files? archives?) could have been exfiltrated.

Last but not least, host forensics could provide you with that answer if you’re lucky enough to access certain artifacts which would help you quantify the amount of data that was sent out (e.g.: SRUM). However, when the data exfil is done on a Windows Server, SCRUM is rarely enabled and therefore, data collected.

A few other ways to answer that question can also be found in answering the “What” one.

### How — How was the data exfiltrated?

The “How” question usually gets answered relatively quickly and/or easily as you investigate the incident. For instance, when performing host forensics on a system and looking at the UserAssist artifacts for a user you know the threat actor used, and find that he executed WinSCP.exe or rclone.exe, you already have the beginning of an answer.

From a host-based perspective, reviewing the processes that were executed during the timeframe of an attack will most often yield the answer to that question. If the exfil was done through a program and/or process that is typically associated with data exfil (such as WinSCP, rclone above), chances are it’ll show up in your forensics.

From a network point of view, you may already be able to answer that question or at least, have an idea on how to answer it if you were able to answer the “How much” one. If you find out that the file server sent 90 GB of data to a remote server outside of the country via SSH, you can then use that information in your host-based analysis to focus on ways a threat actor could exfil data through SSH on a host (ex: WinSCP, rclone, even pure SCP).

Overall, that question is usually one of the easiest to answer, assuming anti-forensics wasn’t performed or the systems on which data exfil occurred were not encrypted. Ransomware can encrypt a lot of artifacts and evidences that would be useful in determining how data was exfiltrated, which can make it easier to answer that question.

### When — At which point in the incident did the data exfil occur?

Another question that can usually be answered relatively easily through the “How much” and “How” questions.

The easiest way to answer it being through network analysis. If you have access to firewall logs and can identify exactly when the first SSH connections to that remote server started and when they ended, you have yourself the timeframe on which the data exfil took place. You can then use that information with other sources of information such as the ISP bandwidth usage and/or download/upload reports. If these reports are broken down by hours for instance instead of days, you may be able to get a more accurate number when it comes to the size of the data that was sent out. In situations where the data exfil started on one day, for instance, on the 15th, and ended on another, let’s say the 16th, if the ISP reports are broken down in 24h periods, you would have to addition together the full size of both days of upload to get an estimate as to how much data, at maximum, could have been exfiltrated.

From a host perspective, determining when the threat actor was active on the compromised system from which the exfil was done from will yield that answer. Depending on the artifacts available, you may be easily able to determine when the data exfil started. For instance, if rclone was launched on the 14th at 4:00 PM UTC, you know that at the very least, this is when the data exfil started. From there, you would need to find another indicator which you could use to determine the end of the exfil. For instance, when the SRU stopped logging information for the rclone process and/or when the ransomware payload was launched. Usually, the data exfil will be finished before the threat actor launches the ransomware. Otherwise, the data they’re trying to exfil would just end up being encrypted, which kinds of defeat the purpose.

### What — The hardest question of all the answer: what data was exfiltrated?

The whole “data exfil” concept aside, by far, the most difficult question to answer. And sadly, the most important one for the victim, but also the breach coach. Your answer to this question, or whatever information you can get to try to answer that question will often determine the next course of action for the victim and the breach coach when it comes to risk assessment (for the exposure) and the notification process.

I’ve never been able to answer that question at 100% on all the incidents I’ve worked on so far. And I doubt that anyone else did (but if you did, please share with me how you do it because that is one impressive feat!). If I was to breakdown what allowed me to answer this question in its entirety, or partially, in situations it would be:

*   No anti-forensics from the threat actor (read: not even cleaning up after themselves)
*   Good ol’ forensics (network or host-based)
*   Unexpected help

The first situation is definitely one of the easiest way to answer that question. In incidents where the threat actor staged the data before exfiltrating it (e.g.: creating ZIP, RAR, 7Z, etc. archives), if the archives are still present when you get called in, all you need to do is grab them and pass them to the victim for its review. This also allows you to answer the “How much” question at the same time, since you have the size of the archive(s), but most importantly, their content.

As far as forensics goes, depending on how the data exfil (and even data staging!) was done, there may be ways to replicate and/or even emulate it with the evidence you’ll uncover. For instance, if the threat actor used a PowerShell command and/or a script to stage the data or exfil it by targeting the exfil process/binary at a folder or server, you can reproduce these commands or run the script (modify it for a “report-only” mode, kind of) in order to get an idea of what data would’ve targeted. This method has caveat obviously in the sense that, depending on the commands that were executed, the time at which they were executed, etc. your results may not yield at 100% the data the threat actor obtained when he executed them at his point in time. For instance, if a rclone command that grabs file that were modified in the last 3 months to exfil them was ran, but you find this it 1–2 weeks after the the fact, you would need to adjust it to target files as accurately as possible. Same thing if you manage to uncover (or recover) a script that was used to staged data by invoking WinRAR to create archives. If you have the WinRAR commands and the folders it targeted, you can replicate it, create the archives and from there, have a good idea of which files would’ve been included in the threat actors’ archives when they were exfiltrated.

Last but not least is … luck (or unexpected help, depending on how you want to see it)! Depending on the environment you investigate, you never know which solutions, configuration and/or logging are in place. And amongst this, there may be something that could assist you in answering the dreaded “What” question. I worked on an incident where an Application Whitelisting solution was deployed on the server where the data exfil was executed from, in learning mode. So while it did not block anything, it did log the 50,000+ files that were accessed by the WinSCP process that was used to send the data to a remote server over SFTP. The lesson to retain here is to make use of anything and everything you have access to. This is why a good understanding of the environment you’re investigating is critical since you may be able to identify something that could help you answering this question.

### Conclusion

As I finish writing this article, I realize that it may be a tad bit longer than what I intended it to be. At the same time however, there is so much more that could be said on the subject and the various questions that each of them could deserve its own article.

In the end, while the data exfiltration angle can be hard to investigate, there is multiple ways to go at it. In a perfect environment where you have a firewall, an EDR, a NDR, etc. answering most of these questions should be a piece of cake. However, the reality is (at least, mine) that this kind of setup is an utopia for the most part, even more so when it comes to SMBs. At most, you’ll have access to decent host-forensics and firewall logs, but that’s it. That’s why knowing how to answer these questions with what you have is important, but even more so, being able to verbalize and explain to the victim and breach coach the limitations and restrictions when it comes to investigating data exfil. That often, you may not get answer to all these questions, nor even one of them.

This being said, hopefully this article exposed you to my bit (and the bit of many other incident responders!) of reality when it comes to dealing with incidents involving data exfil and even given you ideas on how to investigate your own (right now, or the next one(s)!).

Till then, see you next time!