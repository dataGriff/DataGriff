---
title: Configure Hexo
date: 2016-06-27
categories: Hexo
tags: ['Starting Up','Blogging','Hexo']
---

## Configuring Hexo using the \_config.yml file

In the directory where you initalised hexo, you should see a file called \_config.yml file. This is the file you edit to configure your hexo environment. At a bare minimum I did the following to get up and running quickly and efficiently.

I amended the site section at the top of the file to contain my and my site details.

```
# Site
title: dataGriff
subtitle: A blog about data from a guy named Griff
description: A blog with content on SQL, data warehousing, business intelligence and all the rest!
author: Richard Griffiths
language: default
timezone: GMT
```
**Important Note:** It is important to change the language to default as leaving it as "en" was causing the language to be random everytime I generated the site. This was a bug in hexo at the time of writing.

In the URL section I changed the URL to be my custom domain ("www.datagriff.com") and amended the permalink of each post to be just down to the month level and not down to day.
```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: www.datagriff.com
root: /
permalink: :year/:month/:title/
permalink_defaults:
```

In the directory section I just changed the output path to **../Output**, from the default of public, which means the HTML files that get outputted go into a folder in the directory above the source path called Output, as I didn't want the deployment to generate in the same folder as the source. This would cause all sorts of headaches with source control and with the number of git files. I preferred to keep the source code and ouput files separate.

```
# Directory
source_dir: source
public_dir: ../Output
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
```

The only other thing I changed at this point was the deployment details, where I used the git type and entered in my github io repository where I'd be deploying my output.

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/griff182uk/griff182uk.github.io
```

There are a number of other configuration items that I'm sure could be messed around with found at https://hexo.io/docs/deployment.html, but I found the above was enough to get me going.
