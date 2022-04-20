---
layout: post
title: Setting up a Godot Server on AWS
---
> ## If you want to make your first multiplayer game with Godot, I think I can help you.



I make a lot of awful games. But with multiplayer, I can force my friends to play my awful games with me.
<br>
<br>
![test3](https://media.githubusercontent.com/media/urodelagames/urodelagames.github.io/master/photos/netcode-matchy-miners.gif)
_Here's my last multiplayer game, [Matchy Miners](https://urodelagames.itch.io/matchy-miners)._

<br>
<p class="message"> :star2: Here's a step-by-step tutorial on getting a very starter multiplayer project with Godot running in AWS.</p>

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
		- After you create the key pair, it should save a `.pem` file to your computer. Remember that location.
	4. Press `Launch Instance`.

---
> ### Step 5
Configure the EC2 instance on AWS.

1. Click on the EC2 `Instance ID`. It should be named something like `i-0a8f5d7cafdc0a9e6`.
<img src="/photos/AWSInstanceSummary.png" width=819 height=100/>

2. Under the `Security Groups` section, click on the security group named something like `sg-0a7003edc6121a09c`. 
<img src="https://raw.githubusercontent.com/urodelagames/urodelagames.github.io/master/photos/AWSSecurityGroupId.PNG" height=249 width=739/>

3. Click `Edit Inbound Rules`.

4. Click `Add Rule`. Select `All Traffic` for `Type` and `Anywhere-IPv4` for `Source`.
	- :warning: _This makes your server very insecure. But for the purposes for our first multiplayer game, it's fine. Make note that security will have to be tighter for a commercial game._

5. Click `Save Rules`.

6. Repeat steps #3 through #5 for `Outbound Rules`. Start at `Edit Outbound Rules`.

---
> ### Step 6
Upload the Godot server to the EC2 instance on AWS.

Instead of running the server executable on your local machine, we're going to upload the server to AWS EC2 and run it from there.

> :warning: _This next part is much easier with access to a bash terminal. You'll need to run `chmod` permissions commands, `scp` commands to write files to AWS, and `ssh` commands to run Godot on AWS._

1. In the Godot project, export the project as a Linux build (`.x86_64` and `.pck` files).
2. Open your terminal/command prompt on your local machine.
3. Navigate to a folder that has both the `.pem` file and the `.pck` file.
4. Change permissions to `.pem` file. On a bash terminal, run `chmod 400 my-key-pair.pem`.
5. Copy the `.pck` file from your local machine to the EC2 instance using an `scp` command:
	- `scp -i [PEM_FILE_NAME] [PCK_FILE_NAME] ec2-user@[PUBLIC_IPV4_DNS]:/home/ec2-user/`
		- `[PEM_FILE_NAME]` is the path to the `.pem` file
		- `[PCK_FILE_NAME]` is the path to the `.pck` file
		- `[PUBLIC_IPV4_DNS]` is the value called `public IPv4 DNS` on the AWS console.

	<img src="https://github.com/urodelagames/urodelagames.github.io/blob/master/photos/AWSDns.PNG?raw=true" height=402 width=529/>
	
	
---
> ### Step 7
Run the Godot server on the EC2 instance on AWS.

1. SSH into the EC2 instance on AWS.
	- `ssh -i [PEM_FILE_NAME] ec2-user@[PUBLIC_IPV4_DNS]`
	- For example: `ssh -i my-key-pair.pem ec2-54-90-177-85.compute-1.amazonaws.com`
2. Update the EC2 instance and install Godot on the EC2 instance.
	- ```sudo yum install wget && sudo yum install unzip && wget https://downloads.tuxfamily.org/godotengine/3.4.4/Godot_v3.4.4-stable_linux_server.64.zip && unzip Godot_v3.4.4-stable_linux_server.64.zip```
3. Run the Godot server from the EC2 instance:
	- `nohup ./Godot_v3.4.4-stable_linux_server.64 --main-pack godot-network-multiplayer-starter-server.pck &`
4. Disconnect from the EC2 instance.

> ### Step 7
Play the game while connected to your server on AWS!

1. Play the Godot project.
2. Type in your desired name in the `Player Name` field.
3. Type in the `Public IPv4 address` of the EC2 instance in the `Server Host IP` field. 
	- The `Public IPv4 address` can be found on the AWS console and looks like this:
	![test](/photos/AWSipv4.png)
4. Press `Join` and enjoy! Have multiple people join the same `Server Host IP`!


![test2](https://media.githubusercontent.com/media/urodelagames/urodelagames.github.io/master/photos/godot-starter.gif)

---

I'll write another post to explain the netcode!

Thanks for reading. Let me know on [Twitter](https://twitter.com/urodelagames) or [Itch.io](https://urodelagames.itch.io) if you want more. Happy to help the rest of the game dev community.