---
layout: post
title: "Everything you need to know getting started with GitHub and Godot"
tags: Godot, Godot4, Git, GitHub, Tutorial
---

## Introduction

I'm making a YouTube tutorial on getting started with GitHub desktop for your Godot project. I was torn on whether to make the tutorial for GitHub Desktop or the command line. I'm more comfortable using the command line, but the target audience is likely to feel a little overwhelmed by the command line. So I decided the best way to go is to make a video using the GUI and a blog post about the command line.

Git is an incredibly powerful tool. I use GitHub for virtually every project. It keeps a history of every change I save allowing me to easily rollback if I change my mind. It also allows me to easily make the repository public if I decide I want to share my code with the world. Its bread and butter, though, is collaboration. Someone you're collaborating with can pull your changes without losing their own, handle any conflicts if you edit the same file, and submit their changes for review before they're officially added to the project.


 **Table of contents:**
 - [Introduction](#introduction)
 - [Setting up the repository](#setting-up-the-repository)
 - [Common commands](#common-commands)
 - [Common Workflow](#common-workflow)


## Setting up the repository

The first step is to open GitHub and create a repository. Make sure to add a .gitignore file, GitHub has a template for Godot.
![.gitignore Template]({{site.baseurl}}/_images/GitHub-.gitignore-Template.png)

### Clone the repo
Dealing with GitHub is much easier if you setup an SSH key first. [Here's how to generate your key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [here's how to add it to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

1. Clone your repo. Find the URL here, in your GitHub repository. If you setup SSH use the SSH URL, otherwise use HTTPS.
![GitHub Clone URL]({{site.baseurl}}/_images/GitHubCloneURL.png)
    * Enter the command: `git clone <URL>`
2. Create or copy your Godot project 
    * If you already have a Godot project, copy your Godot project into it.
    * If you don't, open Godot and create a new project inside it.

## Common Commands

| Command | Explanation |
| -------- | ------- |
| `git add <filename>` | Mark file to be committed  |
| `git clone <repo url>` | Download a repository for GitHub |
| `git commit` | Commit changes **locally** |
| `git push` | Push committed changes to GitHub |
| `git status` | See what changes have been made |
| `git pull` | Get the most recent changes from GitHub |

## Common Workflow


