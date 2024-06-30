---
layout: post
title: Ballistic arc when you have a set start and end point in Godot
tags: [Godot, Godot4, Ballistics]
---


I added a new tower to my tower defense game recently that launches missiles instead of firing bullets/lasers like my other towers do. Up until this point, all my bullets work just as a raycast, but I wanted the missiles for this tower to have a ballistic arch. I started searching for the best ways to do this and I believe I found a pretty straightforward solution.

Initially, I was trying to follow a Reddit post that goes through the trouble of calculating a sine wave, translating the sine wave to fit the environment, then have the bullet follow it. Oof getting that to work was difficult.

Eventually, I was introduced to bezier curves. Bezier curves take 4 points. P1 and P2 below denote the start and end of the curve. C1 and C2 are control points dictating how much curvature it will have.

![Bezier Curve](/BezierCurve.png)

`Vector3` has a function called [`bezier_interpolate`](https://docs.godotengine.org/en/stable/classes/class_vector3.html#class-vector3-method-bezier-interpolate). It takes those 4 points and a unit of time from 0 to 1, then returns a `Vector3` representing a position along the bezier curve. Translate the object or set it's position to the return value, and you're done.

```
var time_left_ratio = (time_left / total_time)
var p1 = start_pos
p1.y += peak_height
var p2 = target_pos
p2.y += peak_height
var next_position = start_pos.bezier_interpolate(p1, p2, target_pos, 1 - time_left_ratio)
look_at(next_position + Vector3(0.001, 0, 0), Vector3.UP)
position = next_position
```

A consideration to this method is that i
