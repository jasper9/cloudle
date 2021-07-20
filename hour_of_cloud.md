# Hour of Cloud Lab 01 - A Multiplayer Game in the Cloud

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

## Clone the game code to your server

## Connect to your VM instance

## Join your game server

## Allow players to access the game server

## Try joining again


### 🎉  Congratulations!

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
