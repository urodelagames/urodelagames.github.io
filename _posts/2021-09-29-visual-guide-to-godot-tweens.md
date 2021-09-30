---
layout: post
title: Beginner's Guide to Godot Tweens
---

<p class="message">
Tweens are a simple way to tell Godot "I want you to animate this object in my game. This is what the object looks like now and this is what the object should look like after a few seconds have passed."
</p>

Below, I'll provide very simple examples of Godot Tweens using animations from my game, [Hexagourds](/hexagourds).

### Why is it called "Tweening"?
Tweening is short for "in-betweening". When you setup in a Tween in Godot, you provide it 2 "keyframes": 
<ol>
<li> A keyframe that represents what the object should look like at the beginning of the animation and </li>
<li> A keyframe that represents what the object should look like at the end of the animation. </li>
</ol>
When you use a Tween, the Tween automatically generates a bunch of keyframes in between the two keyframes you provided. And it makes the animation look smooth.

Okay I've talked too much.

### Before & After Tweens
I use a Tween to animate the smooth rotation of this Hexagon tile when a player right clicks the tile.
<img src="/photos/CloseUp.PNG" width="452" height="363"/>
<br />
Here's what it looks like to rotate the Hexagon tile 60 degrees *WITHOUT TWEENING*.
<img src="/photos/HexagonRotateNoLife.gif" width="452" height="363"/>
<br />
And here's what it looks like to rotate the Hexagon tile *WITH JUICY TWEENING*.
<img src="/photos/HexagonRotateLife.gif" width="452" height="363"/>

The rotation looks smoother right? And more fun.

### How to use a Tween.
Here's how it works:
```
$RotateTween.interpolate_property(
    rotate_node,
    "rotation_degrees:y",
    (rotation_count-1)*degrees,
    rotation_count*degrees,
    0.2,
    Tween.TRANS_BACK,
    Tween.EASE_OUT
)
```

BLOG POST IS WORK IN PROGRESS.


