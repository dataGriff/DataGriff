---
title: SQL 2012 Admin Exam 01 Prepare Environment
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration', 'Command-Line', 'Virtual Machines']
date: 2016-10-23 13:07:01
---

**Introduction**

So I've decided to take the [SQL 2012 DBA 70-462 Exam](https://www.microsoft.com/en-us/learning/exam-70-462.aspx), it's my last of the three for the MCSA and probably my weakest skill. I've given myself about 40 days from yesterday to get up to speed so this should be fun.

**What this is about**

* Resources
* Environment Setup: How fun this turned out to be!
* Command-Line: What I learned from above.

### Resources

Okay so I'm pretty much going to stick to my tried and tested - get the [certification book](https://www.amazon.co.uk/Training-Kit-Exam-70-462-Administering-ebook/dp/B00IG2OS3Y/ref=sr_1_1?ie=UTF8&qid=1477226838&sr=8-1&keywords=sql+70-462), [book the exam](https://www.microsoft.com/en-gb/learning/certification-exams.aspx) and get to work.
I'll also watch the [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/administering-microsoft-sql-server-2012-jump-start-8259?l=70lJ0hKy_7704984382) stuff and anything else I find of interest on youtube (e.g. [Passing 70462](https://www.youtube.com/watch?v=rWwGPihzgOg)) as I go along.
Based on the book you're also going to need the following downloads:
* [Windows Server 2012 Evaluation](https://www.microsoft.com/en-GB/evalcenter/evaluate-windows-server-2012)
* [SQL Server 2012 Evaluation](https://www.microsoft.com/en-gb/download/details.aspx?id=29066)
* [Adventureworks Database](https://msftdbprodsamples.codeplex.com/releases/view/55330)

### Environment Setup

Based on the book instructions to setup the lab environment to be able to carry out the exercises... I think they expect you to be some kind of infrastructure architect off the bat. I can tell you this, I am most certainly not. This became quite painful for me and I though "Christ I haven't even started the book and this has taken me forever!".
So after failed attempts of creating the environment in [Azure](http://www.sqlservercentral.com/articles/certification/128503/) (Costs and Speed) and failed attempts at doing it myself - I found this wonderful [page](https://toddkleinhans.wordpress.com/) by, who is now a god in my mind, Todd Kleinhans. Thank you so much.

**Just in case you missed it above, here again is the [page](https://toddkleinhans.wordpress.com/) with all the wonderful instructions.**

So if you follow the instructions in the word document attached to the page above you'll be fine.
Highlights from it include:
* Download the trial version of [VMWare Pro](http://www.vmware.com/uk/products/workstation/workstation-evaluation.html), the free version doesn't include snapshots. (yet another time constraint for me to get this done in 30 days!)
* Use [Windows Server 2012](https://www.microsoft.com/en-GB/evalcenter/evaluate-windows-server-2012) instead of 2008 - make sure change from default "update" to custom install during the installation or you'll get stuck in an idiot loop like me.
* As soon as you've installed Windows create shortcuts for administrative tools and command prompt, I found the operating system layout a bit windows 8 and a bugger to navigate through.
* Make sure you tick DNS when installing the Domain Controller - this was a stupid mistake I made as I excitedly rushed through the instructions.
* Make sure click the "new forest" option in Active Directory Domain Services Configuration Wizard when configuring the domain controller.
* Be careful when copying and pasting the instructions for SQLA (yes it says this but I still managed to go on autopilot and mess it up)
* Basically... **READ THE INSTRUCTIONS AND FOLLOW THEM ALL - THEY ARE GOOD.**  
* Although I did have an issue with the SQLCORE installation and had to run the sysprep thing just like the other servers, this wasn't explicitly mentioned in the instructions.
* Share your network drives for resources from your actual machine in the VM to make life easier.

### Command-Line I learned from above

Getting the name of the local area connection - this is not always Ethernet so check!
```
netsh int ip show int
```
Change an IP address.
```
netsh interface ipv4 set address "Local Area connection" static 10.10.10.10 #Change IP Address
```
Okay I knew this one - getting the IP details for the machine you're on.
```
ipconfig
```
Getting the name of the computer.
```
hostname
```
Renaming a machine.
```
netdom renamecomputer %computername% /newname:DC
```
Join a machine to a network
```
netdom join ComputerName /domain:domainname.com
```
Reboot a machine.
```
shutdown /r /t 0
```
I don't think this did anything but may need it if get failed to configure DHCP error...
```
netsh int ip reset
```
