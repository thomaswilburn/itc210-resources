# EC2, WordPress, and Git

## Introduction

This guide will walk you through the process of setting up a server on Amazon EC2 running WordPress, and deploying a theme via Git. You can run a site on this platform under the free tier on Amazon for up to a year, but you will need an [AWS account](http://aws.amazon.com/free) to do so. You can also set up the same deployment process on another server, as long as you have Git installed and SSH access. I highly recommend [Digital Ocean](https://www.digitalocean.com/signups/) for this, since it offers what's essentially a one-click WordPress server image. If you do not go with EC2, go directly to the section marked "Connecting with GitHub".

## Virtual Machines and hosting

Amazon's EC2 (Elastic Cloud Compute) is, at its most basic level, a virtual machine management service. That means that the "server" hosting your site is not a physical box, but a software emulation. It runs a real Linux operating system, and we interact with it as though it's a real computer, but it's actually a program sharing hardware with a bunch of other virtual computers. For web applications, this tends to be efficient and cheap, since most of the time your process is sitting idle between page requests. It's also more secure, since our programs are almost completely isolated from other users - they can't mess us up, and we can't mess them up. And if you need more horsepower, it's easy to spin up duplicate virtual machines to handle the load.

Another advantage of working from a VM is that you can set up your local environment to precisely duplicate your live server. It's much easier to prevent bugs when the PHP version and other infrastructure is identical between environments. For example, I use a Windows PC for development, but run a Linux VM through a program called VirtualBox for my WordPress. Any time I need to start on a new project, I create a new VM to host it, which keeps my setup clean and predictable, and I can easily share the VM with team members running OS X or Linux. You don't have to do this for ITC210, but it's a good habit to get into, and tools like [Vagrant](http://vagrantup.com) can make it much easier to manage.

In this guide, we're going to set up a virtual machine on EC2 that hosts WordPress, pretty much from scratch. Along the way, we'll learn about Linux package management, SSH key pairs, and Git remotes. These are fundamental skills that a modern web developer should have. I am **not** kidding about this: if we could only teach one thing to you at Seattle Central ...well, I would hope it was JavaScript function closures, personally. But the care and maintenance of cloud-based virtual machines is a close second. These are industry-standard best practices, and you are an order of magnitude easier to hire if you understand them.

## The most important step

The following steps are going to seem like a lot of work. They're not actually that bad, and I'll try to put things into context as we go along. But just in case, here's the most important step of the guide, which we're going to refer to as "Step Zero."

* **Step 0:** _Don't panic._ Relax. It's just a computer, and everything can be fixed. When you start getting frustrated, take a step away for a few minutes. If you're stuck for a long time, e-mail me. Do not wear yourself out for hours on end trying to bludgeon your way to a solution (it almost never works).

## Creating your EC2 instance

### Configuring the image

Assuming you have yourself an AWS account, let's get our server running. Inside the [AWS console](https://console.aws.amazon.com/console/home), you'll see a long list of services that Amazon offers to developers. Click on the EC2 link, which should be near the top of the list. Once there, you should see a list of the resources you're using, and underneath that, a big blue button marked "Launch Instance":

![EC2 Dashboard](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/ec2-dashboard.jpg)

### Operating system, sizes, and ports

![AMI Choices](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/ami-choice.jpg)

The first question you face when creating a new server is which operating system you want to install (these pre-built operating systems are referred to as "images"). We're going to go with Ubuntu, which is a stable and common Linux operating system. You can also install other Linux systems, or even Windows, but Ubuntu is a well-known platform, and it'll be easier for you to find help for it. Click the big blue "Select" button to choose the Ubuntu Server 14.04 image.

![Choosing an instance "size"](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/instance-size.jpg)

On the next screen, Amazon lists the "size" of the instance you're going to create. You can stick with a "micro" VM for now - this won't be very powerful, but it will be more than enough for your needs, and you can run it for a year on Amazon's free tier after signing up.

At the top of that screen, it lists all the different configuration stages that you can go through. We're going to stick with the defaults for most of them, but we do need to change the security on this VM, so click on option #6, "Configure Security Group".

![Security groups](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/ports.jpg)

By default, no traffic is allowed in or out of your virtual machine except SSH. Obviously, if we're hosting web pages, we need to open up the ports that the browser uses to talk to your server. Click the "Add rule" button and select the items shown in the picture. Once that's done, you can click "Review and launch" at the bottom to continue, look over the configuration if you want, and then click "Launch" to finally start your machine.

### Keypairs, not passwords

![Creating a keypair](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/keypair.jpg)

Oops! Just kidding, you can't start your machine without creating a "keypair" first. Basically, Amazon does not let you log into your VM with a password, because people usually choose terrible passwords, and then hackers crack into Amazon VMs and use them to mine bitcoins. So by default, you need to download an SSH keypair, which is a long encrypted file that will act as your "password" to the server.

Go ahead and give your key a friendly name, then click the "Download key pair" button to get the file. **Do not lose this file.** Without it, you won't be able to get into your VM, and you'll have to tear it down and rebuild it from scratch. Make a copy of the file for anyone who needs access the server.

Once you have downloaded the file, you can finally press the "Launch instances" button. This time, Amazon will actually start your server. Click "View instances" to go to a screen where you can see all your servers (it should still be booting up). 

### SSH into your machine

![Your instance, initializing](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/instance-list.jpg)

On the right side, the IP address for your server should be listed. Although you can eventually put a real domain in front of your server, you can also access it directly by IP. Let's log in now and finish setting it up. To work with this server, we're going to log in using Secure Shell, or SSH. It's an encrypted channel that connects to a remote machine and lets us use its command line as though we were sitting right in front of it. You will not need a tremendous amount of command shell knowledge to proceed, but it's another key skill that you should make time to learn. I find that William Shotts' [The Linux Command Line](http://linuxcommand.org/tlcl.php) is an excellent resource for learning how a standard shell works. 

#### For Windows

The keypair file that Amazon had you download was a .pem file, which will work directly with a Linux or OS X terminal, but isn't compatible with Windows solutions. We're going to use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to connect to the server, and you'll also need the [puttygen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) program from the same page. Download and run puttygen.exe, then choose File -> Load Private Key and select the .pem to convert. It should show you a pop-up confirming that you've loaded the file, at which point you can click the "Save Private Key" button to create a .ppk file that PuTTY can load.

![Choosing an auth keyfile](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/use-ppk.jpg)

Once you've converted the keypair, open up PuTTY and put your server's IP address in for the host name. Next, switch to the Connection -> SSH -> Auth configuration section as shown above, and load the .ppk file with the "browse" button. Once that's done, you should be able to click "Open" to start your session. On the first connection, PuTTY will ask you to confirm the server's identity with the RSA fingerprint. Just click OK.

Your username for the server is `ubuntu`. The keyfile will be used instead of a password, and then it should dump you out at a command prompt, like so:

![You're in!](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/ssh-login.jpg)

#### For OS X/Linux

On Linux or OS X, the computer comes with an SSH client, and you don't need to download anything. Just open up your Terminal app, navigate to the directory where you saved your keyfile, and run the following command: `ssh -i keypair_name.pem ubuntu@XXX.XXX.XXX.XXX`, replacing the `XXX` portion with your server's IP address. At that point, you should be connected to the remote machine, and you'll see the message of the day (pretty much like the view above for Windows users, but obviously in a different-looking shell).

## Server installation and config

Once you're logged in, we need to install some software. Your VM starts as a blank slate - Amazon doesn't assume that you want to serve web sites with it, or what kind of technology you want to use. That makes sense, because people do in fact use EC2 for all kinds of purposes. But it does mean that the out-of-the-box experience is not great.

We're going to install a standard LAMP (Linux, Apache, MySQL, and PHP) stack. Luckily, software installation on Linux is actually very easy. The `apt-get` command will contact the Ubuntu software repositories, download what we need, and set it up for us. Run the following commands to get your server running:

* `sudo apt-get update` - Asks for the latest Ubuntu package listings, so that we can get current versions of all our programs
* `sudo apt-get install apache2 mysql-server php5 php5-mysql git` - Downloads and installs our server stack, plus the Git software we'll need later

The `sudo` portion of the command is necessary whenever we do anything administrative on the server, and gives us superuser powers. During the setup, MySQL will ask you to provide a password for the root user. Pick something memorable, and share it with your team.

Once it finishes, you should be able to navigate to your server in a web browser by typing its IP address into the address bar, and you'll see the Apache server placeholder page:

![It's alive!](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/apache2.jpg)

## Last steps

We have a couple more things that we need to do before we can close out the command line. First of all, we need to create your MySQL database, which is pretty easy to do from the command line. 

Remember that root password that it asked you to provide during the setup? Let's use it now. Type the following command and hit enter: `msyql -u root -p`. Type the MySQL root password when it asks. Once in the MySQL command line, create the database with the following SQL command: `CREATE DATABASE wordpress;`. Then hit Ctrl-C to exit the MySQL shell.

Next, we need to open the web directory up so that you can upload WordPress. By default, Ubuntu's Apache server puts all web files in `/var/www/html`, but that folder is owned by the `root` user, and you can't upload there. Change ownership to your user by running this command: `sudo chown ubuntu /var/www/html -R`.

## Install WordPress

Installing WordPress on EC2 is really no different than installing it anywhere else, so I won't cover it in detail here, except to say that to log into your server over SFTP, you'll also need to provide the same .pem or .ppk file that you use to log in to SSH. In WinSCP, the Advanced button for a server connection will provide a dialog with an SSH -> Authentication section, and you can select the keyfile there. You will not need to provide a password: the keypair file *is* your password.

## Connecting to GitHub

Let's take a moment and talk about how Git deployment works in theory, before we knuckle down into the practice. Perhaps a charming ASCII flow chart will help:

```
+----------------+      +----------------+      +----------------+ 
|                |      |                |      |                | 
|   Individual   |      |   Individual   |      |   Individual   | 
|   developer    |      |   developer    |      |   developer    | 
|                |      |                |      |                | 
+-------+--------+      +--------+-------+      +-------+--------+ 
        |                        |                      |          
        |                        |                      |          
        +------------------+     |   +------------------+          
                           |     |   |          
                           |     |   |          
                   +-------v-----v---v-------+  
                   |                         |  
                   |       GitHub repo       |  
                   |                         |  
                   +------------+------------+  
                                |               
                                |               
                   +------------v------------+  
                   |                         |  
                   |       Live server       |  
                   |                         |  
                   +-------------------------+  

```

In this chart, each developer works independently on a local WordPress installation, and commits/pushes to GitHub when they have some code done. The repo serves as a shared space for code to live. At regular intervals, someone then goes to the live server and requests the latest version of the code, which will contain all the changes that individual developers have made locally. In other words, the server should always have the latest stable version of the software.

This process has several advantages over the way most students work on WordPress together (i.e., everyone logs in over FTP and edits files directly):

* Using GitHub, there's a running backup of your code, in case you need to restore it at any time.
* When editing over FTP, care must be taken to not edit the same file simultaneously, or two people will overwrite each others' changes. With this workflow, that can't happen.
* Other coders can't break the WordPress installation for everyone else if they have a syntax error, because they're only editing on their own machine.
* It's easy to deploy to a new server when the client signs off, because you can just clone the repo again.

Most importantly, this is how software development works at real companies. If anything, it's more elaborate there: at ArenaNet, we had three "branches" in our source control system, each of which was in ascending order of readiness (dev, stage, and live). A change had to be tested locally, then checked in to test on dev, before it was integrated up to stage and, finally, to the live environment where actual users would see it.

Enough theory, let's get started.

### Create a GitHub repo

The easiest way to start developing is to create the remote repo first, so that you can clone it both locally and on the staging server. In GitHub, create a new repository in one student's account, and choose the option that initializes it with a README file. This repo is going to contain your theme, and only your theme. Add files, such as the standard `style.css` stylesheet, directly to the root folder of the repo. **Do not put the theme in a subfolder of the repository.**

### Authenticate the server with GitHub

Now we need to tell the live server how to connect to GitHub. Like Amazon, GitHub prefers keypairs to passwords for security reasons. Unlike Amazon, we'll need to generate our own keypair file. GitHub [provides a guide](https://help.github.com/articles/generating-ssh-keys/) for this process, but it is a little misleading and primarily intended for people working on a local server, so I'll walk you through the process here. It's still worth reading if you get lost, though.

First, log into your server using SSH. Once you're in, generate the SSH keypair using the following command: `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`, which will generate an RSA-encrypted 4096-bit keypair, and tag it with your e-mail address. The utility will ask you some questions, but just hit enter to use the defaults on each one. It should look something like this:

![Using ssh-keygen](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/ssh-keygen.jpg)

Keys come in two matching parts: a public and a private half. We're going to give GitHub the public half, and keep the private half to ourselves. When we connect, those two parts will match up, and GitHub will know that we are who we say we are.

![GitHub's SSH Key config](https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/github-keys.jpg)

Open your GitHub settings panel, and choose the "SSH Keys" section on the left. Click the "Add SSH key" button, which will show a form with two fields. The first is a nickname that you give to this connection, and can be anything you like ("ITC 298 staging server" is a good choice). The second field is where you're going to paste the public key. In your SSH window, show the key by running this command: `cat ~/.ssh/id_rsa.pub`. You can then select the key with the mouse (everything between the two command prompts, from `ssh-rsa` to your e-mail address) and copy it from there into the browser form:

![Pasting the key into GitHub]((https://raw.githubusercontent.com/thomaswilburn/itc210-resources/master/images/github-paste.jpg)

Click the green "Add Key" button to save this key. Now we just need to test it. In your SSH session, run this command to try to connect to GitHub: `ssh -T git@github.com` (say "yes" when it asks if you want to continue). It should respond like so:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

Good work!

### Cloning your theme

Finally, we need to clone your repository onto the server, so that you can deploy code. Navigate to to your WordPress themes directory (usually `/var/www/html/wp-content/themes`) in your SSH shell, and run this command: `git clone git@github.com:username/repo_name.git theme_name`. This tells Git to connect to GitHub and clone your repo into a folder named `theme_name`. If you look in that folder, you should see the same files that were in your repo, now copied to the server.

From this point on, whenever you want to get the latest code from the repository and deploy it to the server, log in to your server, navigate to inside your groups's theme folder, and run `git pull origin master`. For example, if my group's theme was named "Fancy Pants," my SSH session would look something like this:

```sh
$ cd /var/www/html/wp-content/themes/fancy_pants
$ git pull origin master
From github.com:thomaswilburn/fancy_pants
 * branch            master     -> FETCH_HEAD
Updating 39eddbd..92e70c8
Fast-forward
 header.php      | 4 ++--
 less/menu.less  | 5 +++--
 less/style.less | 8 ++++++++
 3 files changed, 13 insertions(+), 4 deletions(-)
$
```

