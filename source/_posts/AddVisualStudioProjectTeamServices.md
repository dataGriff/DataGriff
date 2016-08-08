---
title: Add Visual Studio Project Team Services
date: 2016-08-07 15:43:15
categories: Visual Studio
tags: ['Source Control', 'Visual Studio', 'Git']
---

The following shows you how to add an existing Visual Studio project to your visual studio team services online account.

First open your solution in Visual Studio, right click the solution and select "Add to Source Control."

{% asset_img VSAddToSourceControl.png Visual Studio Add to Source Control %}

You will then see that the project has green lock icons next to its files.

{% asset_img VSSolutionAddedToSourceControl.png Visual Studio Added to Source Control %}

And the file architecture folder will have git files.

{% asset_img GitAddedToSolution.png Solution Folder Git Files %}

Both showing that you have added the solution to a local Git repo for source control.

Open the team explorer window and select Sync.

{% asset_img VSSync.png Visual Studio Sync %}

With the options that you get presented with, choose Publish to Visual Studio Team Service by clicking Publish Git Repo.

{% asset_img ChoosePublishVSOnline.png Choose Publish Visual Studio Team Services %}

Publish the project to your online visual studio account (you may need to sign in again here), by entering an appropriate name for your new repository and select then Publish Repository.

{% asset_img PublishProject.png Visual Studio Publish Project %}

Open a browser and navigate to your Visual Studio team services homepage. You may not see the project in your list so you might have to choose browse and find your new project there.

{% asset_img BrowseToNewProject.png Browse to Visual Studio Team Service Project %}

There you will see the code from your Visual Studio solution online in your team services source control.

{% asset_img VSOnlineCodeAdded.png Visual Studio Team Service Project Code %}
