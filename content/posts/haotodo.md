---
title: Haotodo
subtitle:
date: 2023-11-18T14:49:34+08:00
draft: false
author:
  name: likai
  link: 
  email: 
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - hugo
  - git
categories:
  - Tech
hiddenFromHomePage: false
hiddenFromSearch: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->
# Hugo to Github

```
hugo new posts/haotodo.md      #再工作目录下 执行新增一个文档 必须以md结尾

hugo server -D       #本地执行，访问localhost：1313 

hugo -F --cleanDestinationDir  #生成public的静态文件，后续这个文件上传到GitHub


git init     #进入到public目录 首次
git add .
git commit -m "first commit v1"
git remote add origin https://github.com/kaivi-duanxin/kaivi-duanxin.github.io.git  #增加远程仓库 注意分支

git push -u origin master  #提交代码到master

如果失败:
git pull origin master --allow-unrelated-histories      #强制同步

或者清空仓库重试
##清空仓库  https://blog.csdn.net/szsc_/article/details/124222118

```

# Linux Hugo deploy
```
#!/bin/bash
Hugo_rootdir=/root/Hugo/KaiviBlogs
current_date=$(date +%d" "%H:%M)
Nginxdir=/var/www/html
NginxUser=www-data


if [ "$1" = "-F" ]; then
    echo "--cleanDestinationDir"
    cd $Hugo_rootdir && hugo -F --cleanDestinationDir
else
    echo "exec hugo command"
    cd $Hugo_rootdir && hugo
fi

if [ $? -eq 0 ]; then
    echo "hugo executed successfully."
else
    echo "hugo failed with exit status $?."
fi


if [ -n "$(ls -A $Nginxdir/* 2>/dev/null)" ]; then
    echo "Files exist in $Nginxdir directory."
    rm -rf $Nginxdir/*
else
    echo "No files found in $Nginxdir/ directory."
fi

cp -R $Hugo_rootdir/public/* $Nginxdir/
chown -R $NginxUser:$NginxUser $Nginxdir/

uptime=`ls -l $Nginxdir/ |awk '{print $7,$8}'|uniq|tail -n1`

if [ "$current_date" = "$uptime" ]; then
    echo $uptime
    echo "Deploy done! http://117.72.33.142"
else
    echo $uptime
    echo "Deploy error!"
fi

```
