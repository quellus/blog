---
layout: post
title: Ballistic arc when you have a set start and end point in Godot
tags: [Godot, Godot4, Ballistics]
---

## Introduction
I'm making a chaotic co-op tower defense game the Godot engine. I recently added a tower that launches missiles. I wanted the missiles to follow a parabolic curve is they flew to their target, but finding tutorials and resources on how to do this was strangely difficult. I hope this post helps anyone trying to do something similar.


## What didn't work
I want to start by explaining what I tried, what didn't work, and why.


### Physics
Probably the most common answer I found when looking up how to make a projectile follow a ballistic or parabolic trajectory in Godot was just to let the physics handle it. If your projectile is a RigidBody with gravity and you launch it with an initial velocity, it will naturally follow are realistic arc.

This solution is perfect for nearly any situation other than mine.
If you just want to throw an object, and don't need to know ahead of time where it's going to land, this is probably the best solution. But what do you do when you have a specific start and end point?

### Bezier interpolate
Eventually, I discovered a function in Vector3 called [`bezier_interpolate`](https://docs.godotengine.org/en/stable/classes/class_vector3.html#class-vector3-method-bezier-interpolate). This solution worked...mostly. The only issue was each missile had a different movement speed. The longer the curve was, the faster the missile flew and the shorter the distance the slower the missile.

Here's an example of how I got it working, but I'll explain more about how this works in the next section.

```
var time_left_ratio = (time_left / total_time)
var c1 = start_pos
c1.y += peak_height
var c2 = target_pos
c2.y += peak_height
var next_position = start_pos.bezier_interpolate(c1, c2, target_pos, 1 - time_left_ratio)
look_at(next_position + Vector3(0.001, 0, 0), Vector3.UP)
position = next_position
```

## The solution
Eventually I came to the conclusion that [Path3D](https://docs.godotengine.org/en/stable/classes/class_path3d.html) is the way to go. The missile, which will be a [PathFollow3D](https://docs.godotengine.org/en/stable/classes/class_pathfollow3d.html) follows the path at a fixed rate. This should solve the issue. My only concern is the overhead of instantiating and freeing a Path and PathFollow so often. I worry the overhead might become an issue as this game grows.

### How bezier curves work
A basic bezier curve uses 4 points. P1 and P2 below denote the start and end of the curve. C1 and C2 are control points dictating how high the peak of the curve is. For a more general description of bezier curves, the control points are the curvature.

![Bezier Curve](/assets/img/BezierCurve.png)

From the perspective of `Path3D`, the pictured curve only has 2 points, P1 and P2. C1 is the `out` of P1 which C2 is the `in` of P2.

For an exmaple, I recommend looking at [`RocketPath.gd`](https://github.com/quellus/godot-tower-defense/blob/6225eaf7fa5a3d22251b3604b1d9259a54461c9e/Ammo/RocketPath.gd)
