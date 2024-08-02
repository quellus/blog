---
layout: post
title: "Everything You Need to Know About Y-Sorting Tilemaps in Godot 4.2"
tags: Godot, Godot4, Tutorial
---

# What is y-sorting and why do I care?
 - Mainly useful for 2D top-down games
 - Hiding the player when it's behind something, like a tree or building

# How does y-sorting work?
 - the parent y-sorts all of its children who have y-sort enabled

# Some of the issues I've faced with y-sorting
 - y-sorting the entire tilemap instead of individual tiles
 - some objects need to be y-sorted differently
    - house y-sorted weird with grass tiles
 - Can't y-sort individual layers

# Recipe for y-sorting successfully
 - Easiest if the player is in the same hierarchy as the tilemap and everything it should y-sort with
 - Enable y-sorting for the parent of all things that need to be sorted
 - Enable y-sorting for each thing to y-sort.
    - Also needs to be enabled for each layer of the tilemap
