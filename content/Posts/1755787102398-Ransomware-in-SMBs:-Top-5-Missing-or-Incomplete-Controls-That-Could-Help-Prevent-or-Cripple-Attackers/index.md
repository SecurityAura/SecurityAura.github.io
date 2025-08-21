---
title: "Ransomware in SMBs: Top 5 Missing or Incomplete Controls That Could Help Prevent or Cripple Attackers"
date: 2025-08-21
draft: false
description: "Quick wins that can help protect or harden SMBs against ransomware attacks."
tags: ["ransomware", "controls", "smb"]
---
## Premise

*[DISCLAIMER: I'm the kind of writer that writes everything in one go. So please excuse any typos or else that might be present. You can always DM me on my various socials to report them and I'll fix them ASAP]*

I think I've posted about these in the post on Twitter (now X), multiple times in fact, but I've always felt that this subject deserved it's own little article. I'm going to try and keep it light, as I don't intend for this article to be a full blown guide on how to address these controls, but just make you aware of them and what's their state in SMBs in 2025.

As an active incident responder, I deal with ransomware every week. Have been doing this for the last few years now, mostly in the SMB realm and I can tell you this: AI-based threats (whatever that even means) is far from being the biggest worry when it comes to ransomware.

Which means, even any AI-based security solution won't help you there when a threat actor gets in via valid credentials, and can just NetExec (or CME for the OGs?) his way through a domain controler before deploying the ransomware payload using PsExec.exe and a targets.txt.

The top missing and/or incomplete controls that could help prevent these attacks, or even cripple attackers hard enough to give defenders enough time to detect them (assuming you're not missing #5 in the list) have, in my opinion, nothing to do with AI and/or whatever kind of security solutions some overhyped vendor tries to sell you.

It comes down to basic IT hygiene and best practices. And we're not even talking about stuff like Microsoft Security Baseline or Active Directory Best Practices. Simply stuff that anyone can do without much overhead (yes, even you MSPs).

## #1 - No MFA on External Remote Service

The initial access vector I probably see the most. May it be VPN, RDP, RD Web or even Microsoft 365/Entra ID (when it comes to BECs), it is hard to imagine that MFA isn't still implemented everywhere in 2025, yet here we are. You know what sentence I hear the most when I get called on an IR (ransomware or BEC) and the threat actor got in via simple username/password authentication?

"We were just about to roll out MFA next week!"

For these organizations that did roll out MFA, I hear:

"But how could this happen! We deployed MFA!"

More on that 2nd one in another blog post (hint: AiTM).

In 2025. Every single week/month, I hear that sentence. Every single day without MFA enabled on an external remote service is another day you may or may not be granted to pursue your business operations in peace. At some point, an attacker is going to get in that way. It's just a matter of time. It's not "luck", it's just that your number hasn't been drawn yet.

Depending on your setup, MFA isn't that expensive to deploy. Even if you have to pay physical TOTP for some users. At the very least, it's less expensive than dealing with a successful ransomware attack. The cost of which depends on whether or not you told your cyber insurance provider you had MFA deployed on your VPN ... when in fact, you didn't.

## #2 - Exposed and/or Unpatched Edge Device

Probably the 2nd initial access vector I see the most (hello CVE-2024-55591!) after the lack of MFA. If there is one category of devices/appliances that should be patched as soon as updates/patches comes out, it's the edge one. It is guaranteed that they'll be targeted the moment a vulnerability and/or POC hits the street for them.

Simply removing the exposition of the management interface (why is it exposed in the 1st place?) could help reduce the attack surface should new CVEs attack it and not another service that is available (e.g.: SSL-VPN).

In the SMB world, the management (read: including the patching) of these devices often falls on the MSP and/or IT partner which installed the device. However, unless the client decides to pay an extra in his monthly or yearly contract, usually, patching will be done on a "best effort" level ... if done AT all. That is, if patches are even available and the client isn't running some EOL edge device.

## #3 - No Basic Network Segmentation

And by basic, I mean REALLY basic. Most organizations do have multiple VLANs or subnets, but they're flat. Workstations can talk to servers on every port, and so can servers.

Rachel from Accounting can VPN in the network while working from home, and the user VPN subnet she lands in allows her to RDP (3389) to the domain controller directly.

So what do you think happen when a threat actor manage to get in through the VPN, using only a username/password (because of #1)? He can attack the servers right away, granted he wasn't lucky enough to login to the VPN with an Active Directory account that already has some access to the servers.

We're not even talking about DMZs here or else, but simple networking:

- Have your workstations in one subnet
- Have your servers in another subnet
- Your workstations should only be able to access specific services on the server subnet (e.g.: port 80/443 for the Web Server, port 1433 for the SQL Server, etc.).
- Your user VPN subnet should be similar to your workstations subnet, if not, even more restricted
- Your servers shouldn't have unconstrained access to the Internet (hi to the person that made a tweet once where everytime he sees a Web Browser installed on a DC, he cringes)

That basic network segmentation can go a long way. The less services/systems you expose, the smaller your attack surface is AND the bigger chances you have of weeding out threat actors in your network that starts lighting up your consoles like a Christmas tree. Assuming you have someone looking at that Christmas tree. More on that in #5.

## #4 - Improper Configuration of "Service" Accounts

I always use air quotes when talking about "service" accounts, because at the end of the day, we all know what they are. Not Managed Service Accounts (MSAs), not gMSA, not sMSA. They are simple Active Directory users (or local users) with the "svc" string in their name. From there, their credentials were plugged in whatever system or service they need to power and we call it a day.

These accounts often have high privileges (why is svcsql a Domain Admin?) and they aren't restricted/limited in what they can do on the network (why can svcExchange RDP to the DC? Why can it RDP AT ALL?). So of course, these accounts are highly interesting for threat actors, because they know that if they compromise only one, it's practically already game over at this point. Somehow that account can be leveraged to compromise an actual DA account, when it can't just straight up RDP to a DC, and then the Kingdom will have fallen.

In an ideal world, you should be using your flavor of MSAs and not actual, simple users. However, if you are, do take the proper steps to secure them. Make it so they can only log in to specific systems, they can't RDP at all, they have long and complex passwords, etc. And if someone ever tries to use them in an interactive fashion (e.g.: RDP), alert on it. But that may be too much to ask, SMBs don't really have a SIEM after all. Subject for another blog post.

## #5 - No AV, NGAV, EDR ... or Even Someone to Look at The Alerts

I have never seen a SINGLE incident in my professional life so far where NO alerts where generated during an incident. Assuming an AV, NGAV or EDR was deployed that is. Even on Windows versions where Windows Defender is built-in now, it'll always have raised alerts. Granted if the organization is running Defender "standalone" (no SCCM, no Intune, no MDE P1, etc.), well, unless the user actually notices the Defender pop-up when there's a detection ... But there will always be a detection. And keep in mind that even when a threat actor disables or uninstall the 3rd party endpoint security solution on a system, Defender will be turned back on. Which means, there are very good chances it'll start triggering on what the threat actor is doing.

Most SMBs don't have an EDR and still run whatever AV/NGAV their MSP or IT provider offers them (hello Webroot, Bitdefender, ESET and all that!). However, it seems like when these products are sold to the client, they're sold to check a bullet point in a list and that's it. No monitoring and/or review of the alerts is done and if there is, they are mishandled anyway (read: the person looking at the alert doesn't understand what he's looking at).

Just having a decent security company do triage on your endpoint protection solution alerts can do a long way. At the very least, they'll know that it's odd for SMB\reception1 to execute mimikatz.exe out of C:\Users\Public\Music. Well, I hope so.

## Conclusion

So if you are a SMB reading this, or even a MSP/IT provider, the best thing you can do right now to help protect your clients against ransomware attacks, or even make it harder for threat actors to execute their goals once they're inside, is to work on these 5 points above. Depending on the environment, some of them, if not all, may be easy to implement, while others may be a bit harder. At the very least, when it comes to #3, should you deploy a network environment with a "basic security" first mindset from the get-go, it makes things way easier down the road.

You are free to do whatever you want with that information and my opinion. At the end of the day, I know I'll continue to deal with incidents where at least one, if not multiple or all of these controls are absent and allowed an attack to go through. So I may as well put it out there for anyone who wants to know what its like to deal with ransomware in SMBs in 2025. So you can see for yourself that there's nothing really "extraordinary" about it.