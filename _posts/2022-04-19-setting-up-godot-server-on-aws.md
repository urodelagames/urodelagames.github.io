---
layout: post
title: Setting up a Godot Server on AWS
---
> ## If you want to make your first multiplayer game with Godot, I think I can help you.


I make a lot of awful games. But with multiplayer, I can force my friends to play my awful games with me. 

Here's a step-by-step tutorial on getting a very simple, starter multiplayer project with Godot running in AWS.

---
> ### Should I know things before doing this tutorial?

Not necessarily.

It would be good if you've created a game using Godot in the past. It's a small starter project, but it uses concepts like instancing, signals, and singletons.

But eh, you should just try the project and poke around - you'll figure it out.

---
> ### Step 1
Download the starter project.

1. Download or clone the project from [the github repo](https://github.com/urodelagames/godot-network-multiplayer-starter).
2. Import the project into Godot.
	1. Launch Godot.
	2. Click `Import`.
	3. Navigate to the `.project` file in the downloaded `godot-network-multiplayer-starter` project.
	4. Click `Import & Edit`.

---
> ### Step 2
Create the server locally.

1. With Godot open, navigate to `res://Scenes/Network.tscn`.
2. Click the root node `Network`.
3. In the inspector, set `Connection Type` to `server`.
4. Export the project as an executable. (`Project > Export > Export Project`)
	- This executable will be your server!
5. Run the executable.
	- Once you run the server, you don't have to do anything else with it. Just make sure its running.

---
> ### Step 3
Run the game locally!

1. In the Godot project, go back to `res://Scenes/Network.tscn`.
2. In the inspector, set `Connection Type` to `client`.
3. Play the project!
4. In the game window, type in your name and the server host.
	- Your name is up to you!
	- The server host will be `127.0.0.1` for now.
		- _`127.0.0.1` or `localhost` represents the computer that the code is working on._
	- Something like this:<img src="/photos/JoinServerUI.png" width="300" height="100"/>
5. Press `Join`.
	- You should now be in the game.
6. Move around with W/A/S/D or Up/Left/Down/Right.
7. Type in chat by pressing Enter.

--- 
<p class="message">
:information_source:
Remember the server we had running in Step 2?
</p>
* Put that window right up next to your Godot game window.
* When you move or chat in the game window, you'll see yourself move on the server!

---
> ### Step 4
Create the server on AWS.

<p class="message">
:warning: This is the part where we put the server actually on the internet, in the AWS cloud. Make sure you undestand how it works locally before moving on.
</p>

1. [Create a free AWS account here](https://signin.aws.amazon.com) if you don't already have one.
The page should look like this after logging in.
<img src="/photos/AWSLogin.png" height="394" width="720"/> 

2. Navigate to `Services > Compute > EC2`.
<img src="/photos/AWSComputeScreenshot.png" height="335" width="250"/>

3. Launch a free EC2 instance.
	- :information_source: An EC2 instance is basically just a server in the AWS cloud.
	1. Click `Launch Instance`.
	<img src="/photos/AWSLaunchInstance.png" height="189" width="240"/>
	2. :warning: Make sure to choose the `t2.micro` instance type. It should say `free tier eligible`. 
	<img src="/photos/AWSAMI.png" height="350" width="770"/>
	3. Create a key pair so that you are authorized to connect to the server later.
		- Click `Create a new key pair`.<img src="/photos/AWSKeyPair.png" height="134" width="532"/>
		- Use these values are press `Create key pair`. <img src="/photos/AWSKeyPair2.png" width="368" height="467"/>
	4. Press `Launch Instance`.

---
> ### Step 5
Configure the EC2 instance on AWS.

1. Click on the EC2 `Instance ID`. It should be named something like `i-0a8f5d7cafdc0a9e6`.
<img src="/photos/AWSInstanceSummary.png" width=819 height=100/>

