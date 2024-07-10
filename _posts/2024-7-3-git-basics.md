---
layout: post
title: "Everything you need to know getting started with GitHub and Godot"
tags: Godot, Godot4, Git, GitHub, Tutorial
---

## Introduction
If you're brand new to Git/GitHub and the command line scares you, I strongly recommend using the GitHub Desktop app. It's a GUI app that makes getting started with Git much easier to use. I made a [video tutorial here](https://www.youtube.com/watch?v=8UrZGJQq5cE&t=2s).

### Why use git?
Git is an incredibly powerful tool. I use git for just about every coding project I work on. It keeps a history of every change I make allowing me to easily rollback if I change my mind. I can start a task, save my changes in a branch, then work on something else without losing those changes. The best part of git, though, is collaboration. It allows you to easily share your code with eachother, merge your changes together, and even submit your changes for approval by your teammates before they get officially merged in.


 ## Table of contents:
 - [Introduction](#introduction)
 - [Setup](#setup)
 - [Common Workflow](#common-workflow)
 - [Common commands](#common-commands)

## Setup
The first step, if you haven't already, is to make a GitHub account. These instructions were designed with GitHub in mind, but will still be useful for git hosts such as BitBucket, or Gitlab.

Next, log in to your account and create a repository.

### gitignore File
Every time you run your game, Godot will create temporary files you don't need. A .gitignore file is important as it makes sure those temporary files won't be uploaded with your code.

If you're on GitHub, there's an option to add a gitignore from a template. GitHub has a pretty good template you can select from the dropdown menu.

![.gitignore Template]({{site.baseurl}}/_images/GitHub-.gitignore-Template.png)

If you're not on GitHub, make a .gitignore file and populate it with this:
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
Dealing with GitHub is much easier if you setup an SSH key first. It's not required, but if you don't, you will have to login to your account every time you want to upload or download changes.

 [Here's how to generate your key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [how to add it to GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

**Warning** make sure you use the .pub file! The other file is your private key and you definitely don't want that being shared!

### Clone the repo
1. Clone your repo. If you setup an SSH key, use the SSH URL you will find here
![GitHub Clone URL]({{site.baseurl}}/_images/GitHubCloneURL.png)
 If you did not setup an SSH key, use HTTPS.

    * Clone the repo by entering the following command with the URL `git clone <URL>`
2. Create or copy a Godot project 
    * If you already have a Godot project, cp the contents of the project into the directory created as a result of the clone command.
    * If this is a new project, open Godot and create a new project in the new directory.

## Common Workflow
Here's the specific order I usually enter commands when working on a project.

```sh
git pull # Downloads the most recent changes from GitHub. I run this every time I start working just in case I forgot I pushed from a different computer or one of my teammates pushed changes

# Open Godot and work on your game. When you get to a stopping point, continue to the next command.

git status # Verify what files you changed

git diff # This will open a text editor showing every line you've changed. Lines you've added show in green and lines you've deleted show in red

git add . # Stages all changed files in the current directory to be commited
git commit -m "Describe what you changed in this commit" # Creates a commit. Your commit history is essentially a changelog of every change you've made. Best practice is to write a clear, but short description of what you changed.

git push origin HEAD # Pushes the commit. Commits are saved locally, and will not be uploaded to GitHub until you push them
```

## The Important Commands
Git has a **lot** of commands, and it would be ridiculous to try to memorize every one. To get started, here's the only commands you need to remember.

| Command | Explanation |
| -------- | ------- |
| `git clone <repo url>` | Download a repository for GitHub |
| `git status` | See what changes have been made |
| `git diff` | See which lines changed in each file |
| `git add <filename>` | Mark file to be committed  |
| `git commit -m "<description>"` | Commit changes **locally** |
| `git push` | Push committed changes to GitHub |
| `git pull` | Get the most recent changes from GitHub |


