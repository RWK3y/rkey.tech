---
title: Setup GitLab Repo
date: 2022-03-03T12:17:04.000Z
draft: false
description: Setup GitLab Repo
---
[Matching Blog Post](/posts/setuprepo)

Set global variable first. 

```
git config --global user.name "Opekktar Yggdrasil"
git config --global user.email "opekktar@opekkt.tech"
```

Then I create a blank project on GitLab. I do not initialize the repo, as that would cause issue when uploading this project for the first time. With the empty repo created I then initialize and upload my project.

```
git init --initial-branch=main
git submodule add -b stable https://github.com/jpanther/congo.git themes/congo
git remote add origin git@gitlab.com:Opekktar/rkey.tech.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

I have and install script I do not want in the repo and I also do not want the public directory in the repo either. So my .gitignore file looks like this:

```
bsh ➜  cat .gitignore
public
.hugo_build.lock
deploy.sh
public.tar
```

Because I use a different theme with my r0bwk3y.com project everything is the same, except the git submodule command.

```
git submodule add https://github.com/sethmacleod/dimension.git themes/dimension
```

and of course I'm pushing to r0bwk3y.com.git instead.
