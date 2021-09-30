---
layout: post
title: Godot Tweens by Example
---

<p class="message">
This blog post gives concrete examples on how to use Godot Tweens.
<br />
Using Tweens, I'll show how you can smoothly animate objects for things like position, color, rotation, and size.
</p>

# ELI5 (Explain like I'm 5) Tweens, Go.
Tweens are a simple way to tell Godot "I want you to animate this object in my game. This is what the object looks like now and this is what the object should look like after a few seconds have passed."
<br />

Below, I'll provide very simple examples of Godot Tweens using animations from my game, [Hexagourds](/hexagourds).

# Why is it called "Tweening"?
Tweening is short for "in-betweening". When you setup in a Tween in Godot, you provide it 2 "keyframes": 
<ol>
<li> A keyframe that represents what the object should look like at the beginning of the animation and </li>
<li> A keyframe that represents what the object should look like at the end of the animation. </li>
</ol>
When you use a Tween, the Tween automatically generates a bunch of keyframes in between the two keyframes you provided. And it makes the animation look smooth.

# Before & After Tweens
I use a Tween to animate the smooth rotation of this Hexagon tile when a player right clicks the tile.
<img src="/photos/CloseUp.PNG" width="452" height="363"/>
<br />
Here's what it looks like to rotate the Hexagon tile 60 degrees *WITHOUT TWEENING*.
<img src="/photos/HexagonRotateNoLife.gif" width="452" height="363"/>
<br />
And here's what it looks like to rotate the Hexagon tile *WITH JUICY TWEENING*.
<img src="/photos/HexagourdsRotateLife.gif" width="452" height="363"/>

The rotation looks smoother right? And more fun.

# How to use a Tween in Godot.

### GDScript Syntax
My tween node in my project has a script attached to with the following code.
Here's how it works:
```
rotateTween.interpolate_property(   # rotateTween is the name of the Tween node. This is telling Godot to create a new animation using this Tween node object.
    hexagonTile,                    # hexagonTile is the 3D tile we want to animate.
    "rotation_degrees:y",           # When the Tween runs its animation, we will change the Y value on the tile's rotation.
    rotation_degrees.y,             # When the Tween runs its animation, it will start at the tile's current rotation.
    rotation_degrees.y+60           # When the Tween runs its animation, it will end at 60 degrees clockwise from the tile's last rotation.
    0.25,                           # This is the amount of time in seconds you want the Tween to run the animation for. This means that on the first run, 
                                        # the Tween's animation will rotate the tile from 0 degrees to 60 degrees in 1/4 of a second.
    Tween.TRANS_BACK,               # This is the animations's "TransitionType". I'll talk about this in the next section.
    Tween.EASE_OUT                  # This is the animation's "EaseType". I'll talk about this in the next section.
)
```

---

### Transition Types and Ease Types
Transition Types and Ease Types are values you can change in Godot Tweens that change the way animations play out. 
* The Transition Type represents what mathematical "shape" the animation uses.
* The Ease Type represents if an animation speeds up or slows down at the start or at the end of an animation.

Here's a useful cheatsheet (Not my original content):
<img src="/photos/tween_cheatsheet.png" width="900" height="700"/>
<br />
In the above (GDScript Syntax) section, I used `BACK` as the Transition Type and `EASE_OUT` as the Ease Type. If you look at graph that says `BACK` on the screenshot, the <span style="color:red">RED line</span> says that our animation will go really fast at the beginning and slow down at the end.
<img src="/photos/back.PNG" width="255" height="255"/>
<br />

This means that when our Tween runs our rotation animation, the hexagon tile will rotate very fast at the beginning and then slow down near the end of the rotation. Notice in the graph that the <span style="color:red">RED line</span> has a huge curve at the top and then comes back down. This makes Hexagon tile rotate slightly too much and then snaps back like a rubberband!
<img src="/photos/HexagourdsRotateLife.gif" width="452" height="363"/>

<br />
<br />
# Other Examples of Tweens in Hexagourds

### Growing the size/scale of an object. e.g. An Apple

<img src="/photos/HexagourdsAppleTween.gif" width="452" height="363"/>
*The Apple grows in size fast initially and then eases out.*
```
tween.interpolate_property(
	apple,
	"scale",
	0.1,
	2.0,
	2.5,
	Tween.TRANS_SINE,
	Tween.EASE_OUT
)
```
<img src="/photos/TransSine.PNG" width="255" height="255"/>
*We're using the <span style="color:red">RED line</span>.*
<br />


---

### Tweening Translation/Position Change of a button on my HUD

<img src="/photos/HexagourdsFinishButtonTween.gif" width="200" height="200"/>
*The finish button notifies the player when it's time to finish the current level by growing and shrinking, back and forth.*
```
# This snippet shows how you can chain Tweens together.
# If you have 2 Tweens, you can make one Tween dependent on the other by using signals (not shown here, but I'll do another blog on signals at some point)
func grow() -> void:
	var normal_scale = rect_scale
	$GrowTween.interpolate_property(
		finishButton,
		"rect_scale",
		normal_scale,
		normal_scale/2.0,
		1.0,
		Tween.TRANS_SINE,
		Tween.EASE_OUT
	)
	$GrowTween.start()

func shrink() -> void:
	var normal_scale = rect_scale
	$ShrinkTween.interpolate_property(
		finishButton,
		"rect_scale",
		normal_scale,
		normal_scale*2.0,
		1.0,
		Tween.TRANS_SINE,
		Tween.EASE_OUT
	)
	$ShrinkTween.start()
```
<img src="/photos/TransSine.PNG" width="255" height="255"/>
*We're using the <span style="color:red">RED line</span>.*


---

### Tweening Color Change of Pumpkin
<img src="/photos/HexagourdsColorChangeTween.gif" width="600" height="363"/>
*The pumpkin's color goes from white to orange - Fast at the start and slow at the end.*
```
tween.interpolate_property(
	pumpkin.mesh.surface_get_material(0), # The orange material on the pumpkin.
	"albedo_color",                       # The color property to animate.
	Color.white,                          # Animate from white...
	Color('ec6f1c'),                      # ... To the orange color
	0.5,                                  # In 0.5 seconds.
	Tween.TRANS_SINE,
	Tween.EASE_OUT
)
```

<img src="/photos/TransQuad.PNG" width="255" height="255"/>
*We're using the <span style="color:purple">PURPLE line</span>.*

---

### Tweening Size of Game Hints

<img src="/photos/HexagourdsGameHintsTween.gif"/>
*The game hints UI window stretches out when needing to notify the player of a new hint!*

```
tween.interpolate_property(
	gameHint,
	"rect_scale",
	0.1,
	1.0,
	1.0,
	Tween.TRANS_ELASTIC,
	Tween.EASE_OUT
)
```
<img src="/photos/Elastic.PNG" width="290" height="390"/>
*We're using the <span style="color:red">RED line</span>.* Notice how the graph looks like it's bouncing and stretching all over the place, that is why the game window feels like its stretching in and out!



---

## Summary and TLDR.
* Tweens are an easy way to animate your game objects and give them life. Use them.
* Experiment with different transitions and easing on the Tweens cheat sheet to find the perfect animation for your game.
* (I think) Every object can be tweened. I use Tweens on 3D objects in my game and for the HUD. Of course Tweens can work in 2D too.
* [Hexagourds](/hexagourds) is a simple game. But adding Tweens to it made it come alive.

--- 

Thanks for reading. Let me know on [Twitter](https://twitter.com/urodelagames) or [Itch.io](https://urodelagames.itch.io) if you want more. Happy to help the rest of the game dev community.