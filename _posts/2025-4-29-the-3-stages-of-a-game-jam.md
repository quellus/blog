---
layout: post
title: "The 3 stages of a Game Jam"
subtitle: "Things I learned about workflow and efficiency during Godot Wild Jam #80"
tags: [Godot, Godot4, Ballistics]
---

## Introduction

To date, I have done 3 game jams (and 2 hackathons, but this post isn’t about that). Two of those game jams were 8 days long, while the other was only 72 hours. So far, I’ve done all of them as a team of two with my good friend and live stream cohost, Code807. I’ve learned a lot from these game jams, and this recent one got me thinking about a few things that I wanted to share.

I want to play my own devil’s advocate here a little and explain that these findings come from the experience of one person on a team of two. You may find that your experience is completely different, or that the things I’m describing don’t work for a solo developer or a larger team. Regardless, I imagine either you’re nervous about doing your first game jam, or maybe you’re a Game Jam veteran looking to for a little perspective on how to improve your process.

I’ve found that each of our game jams have followed a very similar pattern. There have been 3 stages:
1. Brainstorming and initial setup
2. System creation
3. Turning it into a game (and slapping band-aids and duct tape on everything)

## Stage 1: Brainstorming
The first stage mostly comprises of deciding what your game will be. It sets the scene for the rest of the jam because here’s where you set your goals and decide what you’re going to spend the rest of the jam doing. I’m also lumping the initial setup into this stage. Our idea is usually a little vague, and it’s not until we’ve got the repository setup and we’ve started building our first system that we start having a decent idea of what our game will look like in the end.

We’ve tried a few different ways to get organized in this early stage. We’ve tried making a proper Game Design Document, not writing anything down and just winging it, and throwing all of our ideas haphazardly into a Trello board. Some of these ideas worked better than others. Can you guess which ones?

### Game Design Document

When it came to my first Game Jam, I was a little nervous. I knew that in order to make a game fast, we would need to make sure we communicated well and stayed on the same page about what we wanted to build. When I saw a few game developers recommend a Game Design Document, it made sense to use it as a way to organize. I found a found a template and spent the first day of the jam filling it out.

It worked. It wasn't a bad idea at all. The template I used gave us a lot of questions to consider that may not have otherwise. It just did not need to be so formal. Using that method, we made a game called [To The Lake](https://code807.itch.io/to-the-lake).

### Winging it

Well, the second game jam I did *was* successful, but I can't honestly take any credit for that. GMTK was a 72 hour game jam, so we had to move **fast**. My teammate, Code807, had an idea and took off, and all I could do was try to keep up and support wherever I can. In the end, the only thing I can actually take credit for was the implementation of the main menu, emotional support, and tracking down a few assets we needed. For the majority of the jam, I didn't really know what we were building or what we really needed to do to get there. I spent some time prototyping ideas only to find that it wasn't what he wanted to do. We did end up submitting a pretty cool game called [Mystery of Daycart Isle](https://code807.itch.io/daycart-isle). I just can't take much credit for its success.

### Trello/Kanban board

I'm incredibly biased because our most recent Game Jam happened only a week ago from the time of writing this post, but it feels like the most successful of the 3. We're really excited about the game we made, we got really good feedback from it, and we have a lot of ideas about how we would continue developing it. Our process from the beginning and throughout the entire game jam was to throw any and all ideas we had into the Trello board. We had a column just called "ideas" where we could throw all of our ideas for later consideration. This helped us organize our ideas early on, and stay organized every step of the way. One thing that really stood out to me, was moments where we would finish all the TODO tickets, and that's how we knew it was time to have a meeting and sync up on what our next priority was.

Through this process, we made a pretty fun card-based cooking game called [Kitchen Shuffle](https://code807.itch.io/kitchen-shuffle).

## Stage 2: System Creation

This stage seems to take up the vast majority of the game jam. In our game jams so far, it's probably about 75-80% of our time has just been spent creating systems. In To The Lake, we spent a good amount of time creating the interaction system and designing the dialogue system. For Mystery of Daycart Isle, most of the time was spent creating the system to move and scale objects in 3D space. Finally, for Kitchen Shuffle, we spent a day or two creating the system for dragging and dropping cards around, the rest was defining the recipe systems, writing classes to store recipe data and writing functions for the interaction between cooking ingredients.

I often have a tendency to dive in and just make things. It can be a problem especially in a game jam where the time crunch can make it really tempting to just dive in without a plan. My teammate, Code807, really helps keep me in check on this and reminds me often. Our Trello board for Kitchen Shuffle was full of tickets to "create x system", and it wasn't until 2 days before the end of the jam that we actually started putting everything together and actually creating a game.

## Stage 3: Turn it into a game (and hold it all together with duct tape)

Really, the entire game jam is crunch time from start to finish, but the final stretch is a whole new level. This is the important part of this post that I want to draw attention to. Usually, we get to the last 75% of the game jam, and we don't have a game yet. Until this moment, we've spent our time creating systems, creating art, and maybe creating a few scenes that *look* kinda like a game. We haven't actually made a game yet. There's no goal, or no lose condition, things aren't balanced yet. We spend the last day of the jam throwing things together, play testing, balancing, and usually slapping band-aids and duct tape on things.

During our last jam, Kitchen Shuffle, we flipped flopped a few times on how we wanted to handle progression. For most of the jam, we wanted the game to be endless progression. The game would start simple with just a single recipe to cook and a few customers, then over time new recipes and cards will be introduced. Once we got to the final stage of the jam, though, we quickly pivoted. Developing a progression system like that and balancing it well enough to achieve the "chaos" we needed for the jam's theme would have been pretty difficult and time consuming. Instead, we opted to create a level based system so we can iterate quickly. We did have to throw in some band-aids and retrofit a few things to adjust.

## Conclusion

I am hoping to convince you that when it comes to game development, it's not a bad thing if you've put in a lot of work and your game doesn't look like a game yet. I am partially writing this post for myself. In a recent project, I got so focused on trying prototyping and trying to make the game playable as quickly as possible, I didn't give myself time to take a step back and think about what systems I would need. My workflow was to decide on the next game mechanic I wanted to implement, do the bare minimum to make it work, then move on to the next task. I quickly ended up with a lot of technical debt. Every time I wanted to add something new, it would take lot of work to fit it in next to all the previous terrible decisions I've made. There's a lot to take away from our game jam workflow. Even when you're on a really tight time crunch, it's still more efficient to take your time and do things properly.

