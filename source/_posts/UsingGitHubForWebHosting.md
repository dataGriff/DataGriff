---
title: Using Git Hub for Web Hosting
date: 2016-06-27
categories: Github
tags: ['Starting Up','Blogging','GitHub', 'Web']
---

## Creating a Git Hub io Repo

**[GitHub](https://github.com/ "GitHub")** allows you each user to host one website as part of their service using a [github "io" repoistory](https://pages.github.com/ "Github pages").

To do this create a new github repository called [YourUserName]/[YourUserName].github.io.

I used this for my blog and so the URL for the github repository above was what I used in my Hexo config.yml file in the deployment section.

Once you have deployed to the github repository you can view the site using the URL [YourUserName].github.io.

If you have a custom domain you want to use then you need to go to settings of your repo and where it says "Custom Domain" - add your custom domain name and save. This will add a CNAME file to your repository with the name of your custom domain in there.

**Important Note:** In order for this CNAME file to persist, you should create it in your local output folder (with no file name extension), with the name of your custom domain. I found that if I didn't do this everytime I pushed from my local folder the CNAME file would disappear and the connection from my github URL to my custom domain would disappear and I'd get a 404 error, so be aware of this!

![CNAMEFolder](/images/CNAMEFolder.png))

![CNAMEContent](/images/CNAMEContent.png))
