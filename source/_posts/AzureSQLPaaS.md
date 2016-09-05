---
title: Azure Create SQL Database
date: 2016-09-05 19:41:51
categories: Azure
tags: ['Azure','SQL']
---

## Azure Create SQL Database

In this quick example we're going to see how to create a SQL database in Azure using the PaaS offering. This stands for platform as a service and is a complete service provided by Microsoft. It does have it's limitations but is a great way to spin up a database to play with no concerns over infrastructure or software administration. This is any idea for any new applications you may have or just to have some fun!

**What we're going to do:**

* We're going to make sure you have an [azure](www.azure.com) account
* Create a SQL Database in Azure
* Connect to the SQL Database via SSMS
* Cap your spending limit

### Get yourself an Azure Account

Don't fear the Azure! Get yourself over to [azure](www.azure.com) and set yourself up an account. It will ask for bank details but you get a load of credit free at the start and if you have an MSDN license you have even more. You can also cap your spending limit. See the screenshot below to allay your fears... We'll revisit this at the end as I would guess this would be the biggest sticking point for most people when approaching Azure. It's all good...

### Create a SQL Database in Azure

Use search tool and type in SQL Database
Select SQL Database and click Create
Enter database details
Create a new server (if dont already have one)
Configure server
Dont forget you will need this user name and password later!
Set pricing tier to be the cheapest! (basic)
Click create!
It will validate and then eventually create!
Have a look in your notifications and you will see it deploying.
Once it's done, we can look at connecting in the next step!

### Connect to the SQL Database via SSMS
Go to SQL databases
Ensure the firewall bit is done to allows your local IP to connect
Get the full name of your database
Open SSMS and type this in
Ensure SQL authentication 
Hey presto! There's your database in the cloud!


### Cap your Spending Limit
