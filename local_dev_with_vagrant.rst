Local development with Vagrant
==============================

Setting up a development environment, particularly on non-Linux hardware, can be painful - especially if your projects use different languages and databases. To solve this problem, many web developers use Vagrant, a tool for creating, managing, and deleting virtual machines on a per-project basis.

A *virtual machine* is pretty much exactly what it sounds like: it's a simulation of a computer, running as a program inside of an actual hardware computer. We refer to the simulation as the "guest" and the physical computer as the "host." VM support is baked into modern operating systems and CPUs, meaning that they generally run at or near full speed. Using VMs as a development environment has several advantages over developing only inside the host OS:

* Each project is isolated to its own little world, and can't interfere with other programs. This is especially nice when using multiple WordPress projects, since they will probably expect their own database.
* Each operating system can be used for its strengths. I prefer the Windows GUI, but love the power of Linux development tools. With a VM, you can have both.
* Onboarding becomes much easier. New teammates can simply spin up a preset VM and immediately know that everything is already hooked up and ready to go.
* Your development environment can exactly match your production environment, which means you don't run into the dreaded "works on my box" problem.
* Security problems and experimental hacks are isolated. If anything happens to the VM you're using, just throw it away and create a new one.

For this class, I've created a walkthrough that will help you set up a Vagrant box to do your WordPress development. However, once you're comfortable with that, I would encourage you to try creating your own Vagrantfiles. It's a great way to learn about Linux and to test-drive new development stacks like Node, Rails, or Django.

1. Install software
-------------------

You'll need to start by installing the software for your host computer. You'll need:

* `VirtualBox <https://www.virtualbox.org/wiki/Downloads>`_ - Actual VM hosting software
* `Vagrant <https://www.vagrantup.com/downloads.html>`_ - Command-line software for managing VMs
* `GitHub client <https://desktop.github.com>`_ - If you don't already have it
* Some kind of decent text editor

2. Download the Vagrantfile
---------------------------

Grab `this gist <https://gist.github.com/thomaswilburn/be6869e0d647442a1a7e>`_ (either by copying it into your text editor, or by saving the "raw" view) into a file named "Vagrantfile" (no extension). Save this to a folder where you want to work on your project.

The Vagrantfile is a "recipe" for a virtual machine. It's written in Ruby and tells Vagrant about the configuration of the VM: what operating system image to use as a base, and how to customize it once it's up and running. This particular Vagrantfile specifies a machine based on Ubuntu 12.04 with Apache, PHP, and MySQL - a classic LAMP stack. It also sets up a folder to be shared between the guest and host, so you can edit your theme and other files using your regular tools, as if they were normal files on your hard drive (because they are).

3. Start the VM
---------------

Navigate to the folder with your Vagrantfile in the command line (Terminal on a Mac) and run `vagrant up`. This will launch the VM and perform first-time setup operations:

1. If you don't have the ``hashicorp/precise32`` OS image downloaded, Vagrant will go out and get it from the Internet for you.
2. Once the image is installed, it creates a new VM from it and starts it up.
3. Apache, PHP, and MySQL are downloaded from the Ubuntu software repository and installed.
4. Wordpress is downloaded and unzipped into the shared folder for you.
5. The placeholder ``index.html`` page is renamed to ``_index.html``

It'll take a little while the first time you do all this, so plug in your laptop and go get yourself a hot beverage. Hopefully, when you come back, it'll all be done and the command prompt will be waiting for you. 

Your new server is running on http://localhost:8080. Go ahead and visit that page to complete the WordPress configuration process and set up your first account. The MySQL login information will be dumped out to the command line window so that you can fill it in when WordPress asks for it.

You can also see the files that it's using on the webserver in the ``www`` subfolder. WordPress should be installed there already, just waiting for you to work on it.

4. Control the VM
-----------------

Here are some commands that you will find handy:

* ``vagrant up`` - Starts a VM from a Vagrantfile.
* ``vagrant halt`` - Shuts the VM down (as if the power was turned off).
* ``vagrant suspend`` - Pauses the VM ("sleep mode").
* ``vagrant resume`` - Unpauses a suspended VM.
* ``vagrant destroy`` - Completely clears and removes a VM from your hard drive.

Note that none of these commands will affect your shared files. Even if you destroy the VM, your ``www`` folder will still remain, and can be reused if you ``vagrant up`` a new VM in the same folder.

5. Install your theme
---------------------

This assumes you haven't already cloned your repo. If you have, remove it from your GUI and then continue.

Although the GitHub client usually puts your repositories in the same GitHub folder each time, it can actually clone them into any location on your hard drive. Clone your team's repository, and place it in the ``/www/wp-content/themes`` subfolder of wherever you placed the Vagrantfile. If you've done this correctly, and it contains both a ``style.css`` and an ``index.php``, it should show up in the Appearance section of your WP admin dashboard. You'll be able to update it, commit, and pull new changes all from the GitHub application.

6. Profit!
----------

If you're interested in more information about working with Vagrant, their `Getting Started Guide <https://docs.vagrantup.com/v2/getting-started/index.html>`_ is genuinely pretty good. Please also feel free to ask me any questions via e-mail on your setup.
