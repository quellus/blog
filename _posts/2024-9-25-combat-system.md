---
layout: post
title: "How to make a modular combat system in Godot"
tags: Godot, Godot4, Tutorial
---

# Table of contents:
 - [Introduction](#introduction)
    - [What collision means according to Godot](#what-collision-means-according-to-godot)
    - [Bodies vs Areas](#bodies-vs-areas)
 - [The combat system](#the-combat-system)
    - [HurtBox](#hurtbox)
    - [HurtDetector](#hurtdetector)
    - [The Player](#the-player)
 - [How to use this in combat](#how-to-use-this-in-combat)

# Introduction

Getting your enemies and players to take damage when they should isn't easy. Creating a system that can do so in a modular way is even harder. My friend I make games and livestream with, [Code807](https://www.twitch.tv/code807), came up with this system. We've both been using this system for all of our games. It's incredibly simple and will scale easily. I'm currently using it in my open source game with the placeholder name [Cloudedge](https://quellus.itch.io/cloudedge).

[Example project on GitHub](https://github.com/quellus/godot-tutorials/tree/main/combat-system).

This method has only been used and tested in 2D and the tutorial is very focused on 2D, but I fail to think of any reason this wouldn't be useful in 3D as well.

## What collision means according to Godot

If you're new to Godot or game development as a whole, it might seem unintuitive that the game engine doesn't just automatically know when a collision event happens, especially when those two things are physics objects. If I have a drawing of a wall, and I can clearly see my player sprite overlap with it, it might seem intuitive that those things are colliding; however, From the perspective of the game engine, visuals and physics are completely separate. Just having a sprite isn't enough, you also need to tell the engine how it will collide with things and what its physics properties are. Your player character will likely be a `CharacterBody`, your walls and floors will be `StaticBody`, and anything that triggers an event would be an `Area`.

## Bodies vs Areas

Bodies will collide physically, but will not trigger a signal. A `CharacterBody`, or `RigidBody`'s velocity will be changed when colliding with eachother or a `StaticBody`, but these events do not generate a signal. You can only get collision events in a script when an `Area` collides with a body or another Area. 

# The combat system

This system works with 2 `Area2D`s interacting with eachother. One will be HurtBox and its purpose it just to store data about what damage is dealt when something touches it. The other is a HurtDetector and its purpose is to inform the player/enemy/entity when it comes in contact with  

## HurtBox

HurtBox script mainly just stores data for an `Area2D`. From the code perspective, it's just a class called HurtBox with variables storing any necessary information about what kind of damage it deals. For my uses so far, it only stores damage amount, but as the combat system gets more complex, it may store more things like damage type, for element based damage, and information like how strong the knockback is.

Example HurtBox script
```
class_name HurtBox extends Area2D

@export var damage: int = 10
```

## HurtDetector

The HurtDetector's job is to listen to the Area2D's area_entered signal for when it comes in contact with the HurtBox. Then it grabs the data from the HurtBox and emits a signal for the player, enemy, or entity it's attach to to handle.

```
class_name HurtDetector extends Area2D

signal hurt_box_detected(damage: int)

func _ready():
	area_entered.connect(_on_area_entered)

func _on_area_entered(area):
	if area is HurtBox:
		hurt_box_detected.emit(area.damage)
```
## The Player

In the [example project](https://github.com/quellus/godot-tutorials/tree/main/combat-system), the player is a CharacterBody2D with a simple character controller to move around. It listens to its HurtDetector's signal and prints the amount of damage given by the signal. In a real implementation, the player would store its own health and subtract the damage from it each time the HurtDetector's signal is triggered.

# How to use this in combat

This system is really just a building block. At its core, it's just two nodes to use in whichever way fits best for your game. How you use the HurtBox and HurtDetector can vary widely depending on the systems and types of combat in your game.

Here's a few examples on how I use it in my game:
* When the player does a melee attack the animator enables the collider on the HurtBox during a small part of the animation when the sword deals damage.
* When the player casts a fireball spell, the fireball's HurtBox is always enabled until it hits something or dies out.
* The enemies have a constance HitBox, but over time I will rework them to have attack animations similar to the player.
* Something like a spike trap will just be a constant HurtBox.
