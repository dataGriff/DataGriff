---
title: Visual Studio Team Services New Project
date: 2016-07-30 12:26:07
categories: Visual Studio
tags: ['Source Control', 'Visual Studio', 'Git']
---

[Visual studio team services](https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx) is an online tool for managing your visual studio code. It is a free tool for individuals that provides source control, project management, Kanban boards, automated builds, testing and much more. I've found it a useful way of managing my visual studio work and also a way of practicing for team foundation server in the workplace when I don't actually have it!

The following explains how to simply setup a visual studio team services project, clone it in visual studio and then keep your code in the source control provided by team services. I will be choosing the Git style rather than the TFS version of source control, just because I'm now used to Git and I think it's more flexible. Also you get the added bonus of a local git repository if you can't get online for any reason.

## Setup a visual studio team services account
If you haven't already got one - set yourself up a visual studio account. At the time of writing the link for this could be found at [Visual studio team services](https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx), if it's moved just google visual studio team services or visual studio online and follow the relevant paths and instructions.

Another prerequisite for the following is obviosuly having [visual studio](https://www.visualstudio.com/) installed on your local machine... Nearly forgot that one!

## Setup a new online project
Select new under Recent Projects & Teams of you visual studio team services account.

{% asset_img VSOnlineNewProject.png Team Services New Project %}

Give the project a name, a description, pick a process template (**Agile** is fine for me and provides a Kanban board but you may want to choose a different one if you wish) and your version control (as I mentioned in the beginning I use **Git** as I believe it's more flexible), then click create project.

{% asset_img VSOnlineCreateProject.png Team Services Create Project %}

Once your project is created click navigate to project.

{% asset_img VSOnlineProjectCreated.png Team Services Project Created %}

You can start managing your work or add your code. In this example we're just going to focus on getting code in and syncing this with your visual studio project, so we'll choose add code.

{% asset_img VSOnlineProjectChooseCode.png Team Services Add Code %}

## Link your online project to visual studio code
After choosing add code Select "Clone in Visual Studio".

{% asset_img VSOnlineClone.png Team Services Clone %}

In any external protocol request that pops up just click accept. Visual studio will now open with an empty project, but in the **team explorer** window on the right you will see the option to clone from your online project, click clone.

{% asset_img VSClone.png Visual Studio Clone %}

You will see a message for the new repository being cloned successfully.

{% asset_img VSSuccesfulClone.png Visual Studio Successful Clone %}

Then start a new solution for whatever project it is you're going to do. In my example I'm just going to create a windows form project.
Back in the team explorer window click changes and you will see a load of changes as a result of creating the project. Click the + icon to stage these changes.

{% asset_img VSStageChanges.png Visual Studio Stage Changes %}

Then type a commit message in the box at the top to describe your changes and click commit staged.

{% asset_img VSCommit.png Visual Studio Commit Changes %}

Then click sync to share with your server.

{% asset_img VSSync.png Visual Studio Sync %}

Finally in the synchronisation menu that appears select push.

{% asset_img VSPush.png Visual Studio Push %}

You should then see this successfully pushing to your origin which is your visual studio team services project.

{% asset_img VSSuccessfulPush.png Visual Studio Push Success %}

Lets go back online and see if we can see it, refresh the page and... Hey presto your code is added to your online project.

{% asset_img VSOnlineInitialCommit.png Visual Studio Online Initial Commit %}

## Quick Reference for Working with your Visual Studio Team Services
Just to quickly go through the process of adding code, committing, syncing and pushing to your team services project. Make a change to your project e.g. adding a form button.

1. Make a change to your code in Visual Studio
1. Save the changes
1. In **team explorer** go to changes menu
1. Add your staged changes
1. Commit your stage changes with a message
1. Sync your changes
1. Push your changes

{% asset_img VSOnlineCommitedExample.png Visual Studio Online Committed Example %}

This will keep your visual studio code in the visual studio team services online source control, available for you to work from wherever you are with internet access! Sweet.
There's a hell of a lot more you can do with visual studio online but this should give you the basics to use the tool as a system source control.   
