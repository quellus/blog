---
layout: post
title: "Ballistic arc when you have a set start and end point in Godot"
tags: Godot, Godot4, Ballistics
---


I added a new tower to my Godot tower defense game recently. Instead of firing bullets or lasers like my other towers, it launches missiles. These missiles required a creative solution because they have a ballistic arc and a set start and end point.

## What didn't work

### Physics

My first step when I was implementing this tower was to Google. Most results I've found suggest leaving it up to the physics engine. Make the bullet a rigid body, then give it gravity and an initial velocity. The physics engine will generate simulate a ballistic arc for you.

This doesn't work because we have a set start and end point. With the physics approach, the end point of the arc is unpredictable. I could try adjusting the gravity and velocity to get it pretty close to the right end point, but it would be messy.

### Bezier interpolation

THe next idea I had, which I actually implemented, was using Vector3's [`bezier_interpolate`](https://docs.godotengine.org/en/stable/classes/class_vector3.html#class-vector3-method-bezier-interpolate) function. In short, it generates a bezier curve from the points you supply it, you increment a time variable, and the function returns the position on the curve at that point in time. A bezier curves works with 4 points. P1 and P2 below denote the start and end of the curve. C1 and C2 are control points dictating the amount curvature.

![Bezier Curve]({{site.baseurl}}/_images/BezierCurve.png)

The reason this approach doesn't work for me is that the speed of the missile isn't constant. A longer curve results in a very fast moving missile, and the a shorter curve results in a very slow moving missile. I tried adjusting the passage of time based on the distance of the curve, but it was kind of messy and very imperfect.

Here's a snippet of code that may help if you decide to try this approach for yourself.

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

## What did work

My **hopefully** final attempt involved [Path3D](https://docs.godotengine.org/en/stable/classes/class_path3d.html). Everytime the the missile launcher wants to launch a missile, it first generates a Path3D, then spawns the missile as a [PathFollow3D](https://docs.godotengine.org/en/stable/classes/class_pathfollow3d.html) to follow the path. I imagine it's not very efficient to be spawning and deleting nodes so often, but it's the best solution I've found so far.

Here's how it works:

Path3D works on the same principle as the previous explanation. It's a bezier curve, but instead of 4 points, it's generated from 2. The first control point is passed in as the "out" variable of the first point, and the second control point the "in" variable of the second point.

This method also comes with the benefit that the pathfollower automatically rotates itself. I had to rotate the missile manually in the previous solution.
