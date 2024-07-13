---
layout: post
title: "Everything you need to know to get started with GitHub and Godot"
tags: Godot, Godot4, Git, GitHub, Tutorial
---

## Introduction
This post assumes you're already somewhat comfortable with the command line. Git itself is pretty complicated. My aim is only to teach you the commands you need to know to backup your game to GitHub and track your changes. If you're not comfortable with the command line, I strongly recommend using the GitHub Desktop app instead. I made a [video tutorial here](https://www.youtube.com/watch?v=8UrZGJQq5cE&t=2s) covering how to use GitHub desktop without touching the command line.

### Why use git?
Git is an incredibly powerful tool. I use git for just about every coding project even when I'm working alone.

Here are the reasons to use Git:
1. With GitHub, your code is stored in the cloud keeping a backup of your work and making it easy to move your progress between computers or people.
2. Git keeps a history of every change you make allowing you to easily rollback if you change your mind or look back at old ways you solved a problem.
3. Branching (not covered in this post) allows you to work on multiple separate things at the same time and track your progress along the way.
4. Git's biggest strength is collaboration. It allows you to easily share your progress with eachother, and merge your changes together if you both made changes to the same files.


## Table of contents:
 - [Introduction](#introduction)
 - [Setup](#setup)
    - [gitignore File](#gitignore-file)
    - [setup SSH](#setup-ssh)
    - [Clone the Repo](#clone-the-repo)
 - [Common Workflow](#common-workflow)
 - [The Important Commands](#the-important-commands)
 - [What to do if something goes wrong](#what-to-do-if-something-goes-wrong)

## Setup
These instructions were designed with GitHub in mind, but will still be useful for git hosts such as BitBucket, or Gitlab.
1. If you haven't already, make a GitHub account
2. Log in to your account in the browser
3. Create an empty repository
4. Be sure to add the Godot ".gitignore" template when you're making the reposity

### gitignore File
Every time you run your game, Godot will create temporary files you don't need to publish to the repository. The .gitignore lists all of those generated files and directories. As a result, git will completely ignore them.

While you're creating your repository, you should see a dropdown menu to "Add .gitgnore" (see photo below). GitHub has a built in template for the Godot and I suggest starting with it.

![.gitignore Template]({{site.baseurl}}/_images/GitHub-.gitignore-Template.png)

If you weren't able to start with a template, make a `.gitignore` file in the root of your repository. Here are the contents:
```
# Godot 4+ specific ignores
.godot/

# Godot-specific ignores
.import/
export.cfg
export_presets.cfg

# Imported translations (automatically generated from CSV files)
*.translation

# Mono-specific ignores
.mono/
data_*/
mono_crash.*.json

```

### Setup SSH
Dealing with GitHub is much easier if you setup an SSH key first. It's not required, but if you don't, you will have to authenticate every time you upload or download changes from GitHub.

 [Here's how to generate your key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [how to add it to GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

**Warning!** when uploading your key to GitHub, make sure you use the .pub file! The other file is your private key and you definitely don't want that being shared!

### Clone The Repo
1. If you setup an SSH key, use the SSH URL you will find here
![GitHub Clone URL]({{site.baseurl}}/_images/GitHubCloneURL.png)
 If you did not setup an SSH key, use HTTPS.

2. Clone the repo by entering the following command with the URL `git clone <URL>`
3. Create or copy a Godot project 
    * If you already have a Godot project, cp the contents of the project into the directory created as a result of the clone command.
    * If this is a new project, open Godot and create a new project in the new directory.

## Common Workflow
Here's the specific order I usually enter commands when working on a project.

```sh
git pull # Downloads the most recent changes from GitHub. I run this every time I start working just in case I forgot I pushed from a different computer or one of my teammates pushed changes

# Open Godot and work on your game. When you get to a stopping point, continue to the next command.

git status # Verify what files you changed

git diff # This will open a text editor showing every line you've changed. I take this time to look for comments or print statements I meant to delete later.

git add . # Stages all changed files in the current directory to be commited
git commit -m "Describe what you changed in this commit" # Creates a commit. Your commit history is essentially a changelog of every change you've made. Best practice is to write a clear, but short description of what you changed.

git push origin HEAD # Pushes the commit. Commits are saved locally, and will not be uploaded to GitHub until you push them
```

## The Important Commands
These are the only commands you really need to get started. If you open the manpage for git, you'll quickly discover that there's an overwhelming amount. Stick to these for now and you should be okay.

| Command | Explanation |
| -------- | ------- |
| `git clone <repo url>` | Download a repository for GitHub |
| `git status` | See which files have been changed |
| `git diff` | See which lines changed in each file |
| `git add <filename>` | Mark file(s) to be committed  |
| `git commit -m "<description>"` | Commit changes **locally** |
| `git push` | Push committed changes to GitHub |
| `git pull` | Get the most recent changes from GitHub |

## What to do if something goes wrong
[dangitgit.com](https://dangitgit.com/) is one of the most useful websites I've found. I strongly recommend bookmarking it.

Here you will find some common issues and mistakes, and how to fix them.

Also feel free to join my livestreams [twitch.tv/quellusdev](https://twitch.tv/quellusdev) we will happily do the best we can to help you live!
