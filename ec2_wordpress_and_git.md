# EC2, WordPress, and Git in 26 easy steps

## Introduction

This guide will walk you through the process of setting up a server on Amazon EC2 running WordPress, and deploying a theme via Git. You can run a site on this platform under the free tier on Amazon for roughly a year, but you will need an AWS account to do so. You can also set up the same deployment process on another server, as long as you have Git installed and SSH access. I highly recommend Digital Ocean for this, since it offers what's essentially a one-click WordPress server image. If you do not go with EC2, go directly to the section marked XXXX.

## Virtual Machines and hosting

Amazon's EC2 (Elastic Cloud Compute) is, at its most basic level, a virtual machine management service. That means that the "server" hosting your site is not a physical server, but a software emulation that pretends to be a real server. It runs a real Linux operating system, and we interact with it as though it's a real computer, but it's actually a program sharing hardware with a bunch of other virtual computers. For web computing, this tends to be efficient and cheap, since most of the time your computer is sitting idle between the times that it's serving pages. It's also more secure, since our programs are almost completely isolated from other users - they can't mess us up, and we can't mess them up. And if you need more horsepower, it's easy to spin up duplicate virtual machines to handle the load.

Another advantage of working from a VM is that you can set up your local environment to precisely duplicate your live server. It's much easier to prevent bugs when the PHP version and other infrastructure is identical between dev and liv environments. For example, I use a Windows PC for development, but run a Linux VM through a program called VirtualBox for my WordPress. Any time I need to start on a new project, I create a new VM to host it, which keeps my setup clean and predictable, and I can easily share the VM with team members running OS X or Linux. You don't have to do this for the class, but it's a good habit to get into, and tools like [Vagrant](http://vagrantup.com) can make it much easier to manage.

In this guide, we're going to set up a VM on EC2 that hosts WordPress, pretty much from scratch. Along the way, we'll learn about Linux package management, SSH key pairs, and Git remotes. These are fundamental skills that a modern web developer should have, since they are not limited only to Amazon, but will be useful on a number of other cloud hosting providers and for other programming languages.

## The most important step

The following steps are going to seem like a lot of work. They're not actually that bad, and I'll try to put things into context as we go along. But just in case, here's the most important step of the guide, which we're going to refer to as "Step Zero."

* **Step 0:** _Don't panic._ Relax. It's just a computer, and everything can be fixed. When you start getting frustrated, take a step away for a few minutes. If you're stuck for a long time, e-mail me. Do not wear yourself out for hours on end trying to bludgeon your way to a solution (it almost never works).
