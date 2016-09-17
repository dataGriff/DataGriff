---
title: Github IO Page 404 Error Hexo CNAME
categories: Git
tags: ['Blogging', 'Git', 'Source Control','Hexo']
date: 2016-09-17 16:28:56
---

### Introduction

Unless you ensure your CNAME file with the name of your domain is in your github io repository root folder, you'll get a 404 error on the domain of your github io page.
When deploying in Hexo this file gets overwritten unless you place it in the source folder of your raw Hexo files.

**What we're going to do:**

* Ensure the CNAME file is in the Hexo source folder with domain name
* Ensure this CNAME file is the root folder of the Hexo output
* Ensure this CNAME file is in the root of your github io repository

### CNAME in hexo source folder

First ensure that you place a CNAME file in the source folder of your raw hexo folder with the name of your domain contained in the file. **Note** there is no file extension for the CNAME file.

{% asset_img CNAMESourceFolder.PNG CNAME in Hexo Source Folder %}

### CNAME in hexo output root folder

Then after you have done the Hexo generate command, ensure that you can see the CNAME file in the root of your  output folder.

{% asset_img CNAMEOutputFolder.PNG CNAME in root of Hexo Output Folder %}

### CNAME in root folder of Github IO Repository

Then after you have done the Hexo deploy command (using git) to your github IO repository, ensure the CNAME file is there.

{% asset_img CNAMEGitHubIO.PNG CNAME in root folder of Github IO repo %}
