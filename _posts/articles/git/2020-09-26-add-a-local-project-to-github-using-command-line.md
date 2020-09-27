---
layout: post
title: Add a Local Project to Github using Commands
categories: [Git]
tags: [Git, Github]
modified: 2020-09-26
---


In this post, it gives the steps that add an existing local project to your github.
Before we proceed, make sure you have an Github repo created, let's say,

```bash
https://github.com/username/my-project.git
```

---


##### Step 1 
Open terminal, and change the current work directory to the local project. For example
```bash
$ cd ~/my-local-project
```

##### Step 2
Initialize the local directory as a git repository.
```bash
$ git init
```

##### Step 3
Add the project files into the local repository
```bash
$ git add .
```

##### Step 4
Commit the staged project files into the local repository
```bash
$ git commit -m "Commit local project"
```

##### Step 5
In terminal, add the remote repository URL to your local repository.
```bash
# Add
$ git remote add origin https://github.com/username/my-project.git
# Verify
$ git remote -v
```

##### Step 6
If your github repository is not empty, then pull first
```bash
$ git pull origin master --allow-unrelated-histories
```

##### Step 7
Push project in local repository to github.
```bash
$ git push origin master
```

Hope it helps!