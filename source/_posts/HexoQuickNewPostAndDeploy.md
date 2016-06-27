---
title: Hexo Quick New Post and Deploy
date: 2016-06-27 18:11:55
categories: Hexo
tags: ['Hexo','Git','Blogging']
---

Below shows the quick process of creating and deploying a new post, while keeping everything under source control. This post assumes you already have the source folder initialised with git and linked to a standard repository, and the output folder initialised with git, linked to your io repository.  

Navigate to your hexo source folder in node.js command prompt.
In my case this is E:\\Documents\\Projects\\DataGriff\\Raw.


![Hexo Source Folder in Node.js Command Prompt](\images\HexoSourceFolder.png)

Enter the command:
```
hexo new post test
```
![Hexo New Post Command in Node.js Command Prompt](\images\HexoNewPost.png)

This will create a new post file called test.md. Don't close the node.js command prompt as you use it again at the end to deploy.

Start Git bash on your source folder and check the status by using:
```
git status
```
![Git Status New Post Added](\images\GitStatusChange.png)
This will show a new file called test.md has been added.

Use the command below to add all the changed or new files.
```
git add .
```
![Git Add All Files Using Git Bash](\images\GitAddAll.png)
Use the command below to commit the changes with a message.
```
git commit -m "new post added"
```
![Git Commit with Message  Using Git Bash](\images\GitCommit.png)
Check the status again and you will see there are no changes now as you have committed them.
```
git status
```
![Git Status Clean  Using Git Bash](\images\GitStatusClean.png)
To keep your remote repository up to date, use the command below.
```
git push origin master
```
![Git Sync with Remote Using Git Bash](\images\GitPushOriginMaster.png)
Revert back to the node.js command prompt and use the following command to generate the output files
```
hexo generate
```
![Hexo Generate Command in Node.js](\images\HexoGenerate.png)
Still in the node.js command prompt use the following command to deploy the output files to your github io repo and so your website.
```
hexo generate
```
![Hexo Deploy Command in Node.js](\images\HexoDeploy.png)
You should then be able to see the new post if you refresh the page.
![Hexo New Post](\images\HexoTestPost.png)
