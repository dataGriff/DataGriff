---
title: Azure Create SQL Database
date: 2016-09-05 19:41:51
categories: Azure
tags: ['Azure','SQL','Starting Up']
---

## Azure Create SQL Database

Please note if you want to go straight to the official azure documentation, please go [here](https://azure.microsoft.com/en-gb/documentation/articles/sql-database-get-started/). It's probably more up to date than this already! :)

In this quick example we're going to see how to create a SQL database in Azure using the PaaS offering. This stands for platform as a service and is a complete service provided by Microsoft. It does have it's limitations but is a great way to spin up a database to play with no concerns over infrastructure or software administration. This is any idea for any new applications you may have or just to have some fun!

**What we're going to do:**

* We're going to make sure you have an [azure](https://azure.microsoft.com/en-gb/) account
* Create a SQL Database in Azure
* Connect to the SQL Database via SSMS

### Get yourself an Azure Account

Don't fear the Azure! Get yourself over to [azure](https://azure.microsoft.com/en-gb/) and set yourself up an account. It will ask for bank details but you get a load of credit free at the start and if you have an MSDN license you have even more. You can also cap your spending limit. See the screenshot below to allay your fears... We'll revisit this at the end as I would guess this would be the biggest sticking point for most people when approaching Azure. It's all good...

### Create a SQL Database in Azure

When logged in to your Azure portal use search tool and type in SQL Database.

{% asset_img 01_Search.png Search SQL Database Azure Portal %}

Pick SQL Database

{% asset_img PickSQLDatabase.PNG Pick SQL Database %}

then click Create

{% asset_img 02_Create.PNG Create SQL Database Azure %}

Enter in the  database details. This includes the database name, setting up a new resource group (if don't already have one), setting your collation and selecting your developer pricing benefit package. Remember to select the basic pricing tier to save your credits. You'll also need to configure a logical server for this database to live which you will see in the next section.
P.s in the Select source section you can choose a sample database rather than a blank one if you want some ready made data!

{% asset_img 03_ConfigureDatabase.PNG Configure SQL Database Azure %}

Click the Configure Server settings and create a new server (if don't already have one).

{% asset_img 04_ConfigureServerSettings.PNG Click Configure Server Settings Azure %}

{% asset_img 05_CreateNewServer.PNG Create Logical Server Azure %}

Set the name of your logical server. Create a server admin login and a password, **you'll need these later to logon to your database so remember this!** Leave the rest as default then just to get along...

{% asset_img 06_ServerDetails.PNG Configure Logical Server Details %}

Now that you are back to your configure database page after configuring your server, click create! Have a look in your notifications and you will see it deploying.

{% asset_img 07_DeployStartedUnderNotifications.PNG Validating Deployment %}

Once this is done, we can look at connecting in the next step!

### Connect to the SQL Database via SSMS
In Azure go back to your homepage and pick SQL databases. You will see the list of your SQL databases. Pick the one you want to create a connection to (the one you just made).

{% asset_img YourSQLDatabases.PNG Go to your SQL Databases %}

Now you need to ensure that your local PC is going to be able to connect to your database so click the Set Server Firewall tab at the top in the database settings.

{% asset_img SetServerFirewall.PNG Set Firewall %}

Click add client IP.

{% asset_img AddClientIP.PNG Add Client IP %}

This will then save your IP address to allow connection.

{% asset_img SaveClient.PNG Saving your IP in Progress %}

And finally you will get a success...

{% asset_img FirewallSuccess.PNG Firewall Success %}

Now you have set the firewall rules, go back to your database settings and get the full name of your database. This is the one that ends with database.windows.net and you can hover over the end of it and just click copy.

{% asset_img CopyServerName.PNG Copy full path of database %}

Open SSMS and paste in the full path, ensure SQL authentication is chosen and use the admin account and password you created earlier.

{% asset_img SSMSConnect.PNG SSMS Connect Dialog %}

Hey presto! There's your database in the cloud!

{% asset_img SSMSConnected.PNG SSMS Connected to database in the Cloud %}
