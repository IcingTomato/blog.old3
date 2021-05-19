---
layout: default
title: "Plan of Jekyll Blog Transfer - Episode 6: Git, Shell and Crontab"
tags: software tutorial
---

# Git setup

Everytime I finish my blog, I have to push to two different platform: [GitHub](https://github.com/) and [Gitee](https://gitee.com/).

[Gitee](https://gitee.com/) is a collaboration platform for software development & code hosting in China Mainland.

Because I use Ali ECS Area Hangzhou, it's hard to visit GitHub, so I have to use Gitee.

So

```shell
cd 'your-repository-path'
git remote add gitee 'your-gitee-repository-address'
# Push to 'gitee'
git push gitee master    
# Push to 'github'
git push origin master   
```

You can also change **'your-repository'/.git/config**

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = 'your-github-repository-address'
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[remote "gitee"]
	url = 'your-gitee-repository-address'
	fetch = +refs/heads/*:refs/remotes/gitee/*
```

# Shell and Crontab

I wrote a shell script to simplify the process of generating.

```shell
root@mail:~# cat push_blog.sh 
#!/bin/bash

cd ch1hfe1.github.io/

git pull origin master

jekyll build -d /var/www/html/
```

And I also wrote a crontab.

```shell
root@mail:~# crontab -u root -l
0 0 * * * bash /root/push_blog.sh
```

On 0 o'clock everyday, it will do this job.

Maybe this blog will auto publish by crontab? I don't know.

# How to use crontab

```
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- Weekday (0 - 7) (Sunday is 0 or 7)
|    |    |    +---------- Month (1 - 12) 
|    |    +--------------- Day in a month (1 - 31)
|    +-------------------- Hour (0 - 23)
+------------------------- Minute (0 - 59)
```
```
*/5 * * * * reboot  # It means every five minutes reboots the machine.
```

OKay, **git pull** is working, but **jekyll build** doesn't work.
