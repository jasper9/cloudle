# Hour of Cloud Lab 01: A Multiplayer Game in the Cloud

*This is an example [NEOS Tutorial](https://g3doc.corp.google.com/cloud/gettingstarted/tutorial/g3doc/index.md?cl=head)*

<walkthrough-watcher-constant key="project_id" value="<your-project>"></walkthrough-watcher-constant>

##
![Create a Linux virtual machine in Compute Engine](https://walkthroughs.googleusercontent.com/content/images/compute.png)

**Overview**

To run a game in the Cloud, you need a dedicated server, plenty of RAM, and plenty of bandwidth. Why not let Google Cloud handle these requirements for you? In this lab, you'll learn how to install, configure, and run a multiplayer IO style game on Compute Engine.

> *Note: This lab is not approved by or associated with the game developer.*

Your game server software will run on a [Compute Engine](https://cloud.google.com/compute/docs/instances) instance, which is a [virtual machine](https://wikipedia.org/wiki/Virtual_machine) that runs on Google's infrastructure in a Google Datacenter. You will create your cloud server and run Linux on it.

### What you'll do

1. Create a Compute Engine virtual machine instance
1. Install NodeJS, a programming language based on Javascript
1. Deploy the games source code to the server
1. Configure the network to allow anyone on the internet to access your game server and play


Estimated time: <walkthrough-tutorial-duration duration="60"></walkthrough-tutorial-duration>

To get started, click **Start**.


## Enable Compute Engine in a Cloud project

1. <walkthrough-project-setup></walkthrough-project-setup>

1. <walkthrough-enable-apis apis="compute.googleapis.com"></walkthrough-enable-apis>


## Create and configure your Compute Engine instance

1. In the Cloud Platform Console, go to the VM instances page by clicking the **Navigation menu** button > **Compute Engine > VM instances**.

   You can see where it is by clicking the following button:
   <walkthrough-menu-navigation sectionId="COMPUTE_SECTION"></walkthrough-menu-navigation>

1.  Click **Create instance**.
    <walkthrough-spotlight-pointer cssSelector="[instrumentation-id=gce-zero-new-vm],[instrumentation-id=gce-vm-list-new],[id$=create]">Show me</walkthrough-spotlight-pointer>

1.  On the **Create an instance** dialog, configure the following for your instance. Ignore the other values for now.
Field  | Value
-------| --------
Name   | game-server
Region | us-central1
Zone   | us-central1-f

That's it for basic configuration! Click on Management, security, disks, networking, sole tenancy to show advanced options.

### Tag your instance
Now click the **Networking** tab, add the tag **game-server** to the **Network Tags** field. Later in the lab, you'll use this tag to create a firewall rule to allow gamers to access your server.

### Set up a static IP for your instance
To forward incoming requests to your instance dependably, your instance needs a static IP. Still in the **Networking** tab, click in the **Network interface** field and select **External IP > Create IP** address from the dropdown. Name your IP address **game-ip** and click **Reserve** to create the address.


Click **Done** to close this dialog.


Click <walkthrough-spotlight-pointer spotlightId="gce-submit">**Create**</walkthrough-spotlight-pointer> to create your new instance.

This action will take you back to the VM instances page.

> **Note:** It can take up to 20 seconds for your instance to be created.

Once your instance is created, take note of it's **External IP Address**, write it down. You are going to need it in a little bit. This is the address of the server on the Internet and how you and your friends will connect to the server with your browser. It will be something like 35.202.167.99, but will have different numbers.

Click on **SSH**. You will run all the commands in secure shell (SSH).


## Install NodeJS on your instance

The game we are deploying is written in Javascript using the NodeJS framework. We need to install NodeJS and its package manager NPM on the server so we can run the game code.

**Set up NodeJS, NPM & Git**

The game server runs on top of [NodeJS](https://nodejs.org/), so it requires the NodeJS server to run. We can install NodeJS using the package manager for the Operating System. On Debian (And other Linux variants) the Apt Package Manager is used to easily install software on the system. It takes care of installing the software you need, and any dependencies the program may have automatically. Dependencies are the other pieces (programs, libraries) that a given program needs to function.

You'll want to update the software repositories on your system before installing anything. This will ensure we are getting the latest and greatest packages when we install something. We do this with the update command. Run the following:

~~~
sudo apt-get update
~~~

Let's take a look at this command in a little more detail. Sudo is a command that you can put in front of any other command to change who the command is run as. Normally when you run a command it executes as you, or the user you are logged into the computer as. Since we are installing a system program that we want to be available for anyone, we need to install it as whats known as a priviledged account, or as root. Root on Linux is the Super User, the user that can do anything on the machine including blow everything away irrepairably, so we always need to be careful when running commands as root.

Now that we have updated our repositories, we can install NodeJS and NPM on our server. We are also going to install Git, which is a command used to clone the games code from a source code repository.
~~~
sudo apt-get install -y nodejs npm git
~~~
## Clone the game code to your server
Now that you've setup NodeJS you need to clone the source code for the game to your server so you can run it. The code is stored in a public repository called GitHub. Software developers use [GitHub](https://github.com/) as a place to store, manage and share their code with their teammates or with the world at large.

You clone the software to your server using the following command
~~~
git clone https://github.com/vzhou842/example-.io-game.git
~~~

This copies the source code onto your system in the folder that you run the command from, or in this case your Home directory.

If you are interested in how the game code works, [this walkthrough](https://victorzhou.com/blog/build-an-io-game-part-1/) will take you through the code in detail.

After cloning the software to your server, you need to install any of the dependencies the software developer has used in building their application. Dependencies are other pieces of code that provide functionality to the developer that they can reuse, rather than having to create themselves. NodeJS uses a tool called NPM or Node Package Manager to install the dependencies for you. You need to change to the directory you just cloned the code into and then run the NPM command to install the dependencies.

~~~
cd example-.io-game
npm install
~~~

NPM will download and install all the dependencies required to run the game. this will take about 30 seconds or so. We are now ready to build and run our game!

Building your game
~~~
npm run build
~~~

Now lets run your game.
~~~
npm start
~~~



## Join your game server
If your game successfully started, you should see a message that says Server listening on port 3000. Now open a new tab in your browser and connect to **http://<External_IP>:3000**. You'll type in something like **http://35.202.167.99:3000**

This tells your browser to connect to the server at **35.238.186.255** on **Port 3000**. Ports are like doors into the server. The server listens for connections on ports. Different ports are used for different things. For example, computers send email between one another on port 25, web sites listen on port 80.

## UGH IT'S BUSTED!
Your browser wasn't able to connect to your game server? What gives! This is because the network firewall is not allowing any connections to the server, even if it is listening. The network is protecting the server from any malicious attackers who might be seeking to hack your server and steal your stuff. We need to open up the network on port 3000 so that the server can receive connections.

## Allow players to access the game server
In order to allow remote gamers & their web browsers to connect to your game server, you'll need to create a firewall rule. By default, the network blocks outside connections in order to protect your server from being attacked & hacked. To setup a firewall rule you need to:

1. In the Cloud Platform Console, click the **Navigation menu > VPC network**.
1. In the **VPC Networks** section, click **default**.
1. Then click the **Firewall rules** tab.
1. Click the **Add firewall rule** button the following dialog opens:

1. Configure the fields below. Ignore the other fields.

Field  | Value
-------| --------
Name|game-rule
Targets|Specified target tags
Target tags|game-server
Source filter|IP ranges
Source IP ranges|0.0.0.0/0
Protocols and ports|Specified protocols and ports: tcp:3000

>**Note:** By default, the game server (Well, really NodeJS) uses 3000 as its default listening port. You can change this port number, but we don't need to worry about this for our activity.

Click **Create** to create your new firewall rule. Users can now access your server from their local Minecraft clients.


## Try joining again
Try connecting to the address (http://<external_ip>:3000) of your game server again. This time you should be presented with the games opening screen. Share this address with some of your classmates or friends and you should all be in the game together.

## 🎉  Congratulations!

You did it! You set up your IO Game Server on Compute Engine.

## End your lab
When you have completed your lab, click End Lab. Qwiklabs removes the resources you’ve used and cleans the account for you.


### Do more with Compute Engine

<walkthrough-tutorial-card id="transfer_files_to_gce_instances" icon="COMPUTE_SECTION" keepPrevious="true">
**Transfer files to your VM instance.** Different options are available
depending on your workstation OS and the target instance OS.</walkthrough-tutorial-card>

<walkthrough-tutorial-card url="https://cloud.google.com/compute/docs/tutorials/basic-webserver-apache" icon="COMPUTE_SECTION">
**Set up a basic web server.** Learn the basics of running an Apache server on a
VM instance.</walkthrough-tutorial-card>
