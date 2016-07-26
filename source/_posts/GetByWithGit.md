---
title: Get by with Git
date: 2016-07-24 16:21:48
categories: Git
tags: ['Source Control', 'Git']
---

Source control allows you to keep version history of your code all in one
place. It also allows multiple teams to work on the same code using the
concepts of branching and merging... I'm not going to talk about that
though - this is just a brief post on how you can easily set up source
control for free for your own code, using it locally (on your machine), with the option of
pushing it to a distributed [github repository](https://github.com/) as well, along with
 the key commands to get by for most of what you're going to do.

## Quick Reference
This is what you're going to be doing most of the time! (once setup).

* Add your local files
* Commit your local files
* Push those changes to your distributed github repo.

```
git add .
git commit -m "message"
git push origin master
```
For detail see sections below.

## Step 1: Setting Git up Locally

First download [git bash](https://git-for-windows.github.io/). This allows you to locally set up source control on
a folder on your local machine (hooray!).
To start git bash on a folder after you have installed git, just right-click the
folder and select "Git Bash Here".

![Git Bash Start](/images/GitBashStart.png)

Git bash just looks like a command prompt.
Below is an example of some git bash code.

![Git Add Example](/images/GitAdd.png)

## Step 2: Commands to Get By Locally
The following command initialises git on the folder you started git bash in.
That's all you need to start a local git repository! You're only going to
execute this once, when you're setting up your new repo locally.
```
git init
```
The following command tells you the status of the local git folder. What has changed,
what hasn't been committed etc. It's always handy to check...
```
git status
```
I pretty much run this as standard every time before I perform a commit just to
make sure everything is added to the git source control. The status will changed
as you amend files.
```
git add .
```
You can though add a specific file name if you only want to perform that commit
right now. I rarely do this though as I just want everything chucked in so I
perform the command above. **Remember you can only commit files to your git
source control once you've added them!**
```
git add "filename"
```
Once you've added or amended your files commit them to the git source control
with an appropriate message using the following.
```
git commit -m "My Message"
```

## Step 3: Commands to Get by with Distributed Git
First you need to set up your repository on your [github](https://github.com/)
and then copy the link in your repo where it says clone or download.

![Github Copy URL](/images/GitHubCopyURL.png)

When in git bash on your local folder run the following command to add the github
repository link, pasting in your repo where it says "add origin".
```
git remote add origin "http://github.com/foo"
```
At the start it is likely you will have a readme file in the github repo, so you
will need to first pull from the Distributed repo to make sure everything is in sync,
otherwise it just throws a wobbly. So run this.
P.S when you run this you'll probably get a text editor pop up asking for a
commit message - I just close this.
```
git pull "http://github.com/foo" master
```
Then this is the command you will be running all the time from your local
folder to keep your distributed repo up to date. It's pretty much git add .,
git commit -m "message", then the following command for most of your source control
needs.
```
git push origin master
```
You can see when you execute the above command the local changes get pushed to
the distributed system. In the screenshot below the test.txt file had been
pushed to the distributed repo from the local folder.

![Github Repo Pushed](/images/GitRepoPushed.png)

If you have made changes to the distributed repo and want to bring them in to
your local git repo, you can just run this.
```
git pull
```
