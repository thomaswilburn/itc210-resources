Setting up your GitHub repos
============================

If you haven't worked extensively with Git, it can be confusing at first. This guide will walk through a simple series of steps you can take to get your repo set up. There are many other ways to initialize and synchronize a repo, but I think this is the easiest path if you're inexperienced. It's split into two steps: first the initial repo setup, followed by the configuration for contributors.

I will assume through this guide that you're using the graphical Git clients, either for Mac or Windows. You do not need to use the command line for this, although familiarity with the command line will help tremendously with your experience. I'm also going to assume that you have a local WordPress install running.

Initial setup
-------------

We're going to create our repositories using the GitHub website, just because it's the same no matter which OS you're on. Use the ``+`` button in the upper-right hand side to create a fresh repo, named whatever you want. *Make sure* to check the box marked "initialize this repository with a README." That will allow us to clone it immediately.

Once the repo is created, the site will show you some instructions for pushing to that repo. Ignore them. Instead, click the "clone" button on the right side of the repo's web page. This should open the GitHub for Windows/Mac application, and ask where you want to save the cloned repository. Find your WordPress installation, and choose the ``wp-content/themes`` folder as the destination. This will create a new folder inside of ``themes`` with the README file from your newly-created repo. Go ahead and update the readme, then commit and sync to make sure that the connection is solid.

Collaborator setup
------------------

Once the initial repo is set up, it's much easier to work with GitHub. Just click the clone button on the repo web page, then choose your ``wp-content/themes`` folder for your local web install when the GitHub client asks where you want the repository. From there, you should be all set to make changes, commit, and sync. 

Working together on GitHub
--------------------------

One of the advantages of Git is that it provides tools for managing conflicts: those times when two people make changes to the same file, then try to commit them both. Unfortunately, the graphical GitHub clients are not always very good at dealing with conflict. I'd like to propose the following workflow instead, which takes advantage of Git's lightweight *branches*.

A Git "branch" is an alternate version of the repo, isolated from any changes that are going on in the master branch. Whenever you start work on a feature, you should create a new branch. For example, if I were going to add CSS for a staffing page, I might use my GitHub client to create a "staff-css" branch. GitHub for Mac has a dedicated Branches tab that you can use to create this branch. On Windows, use the pulldown menu marked "master" (for the default branch) to create a new branch, just by typing its new name.

On my branch, now you can do whatever work I want, without worrying about what anyone else is doing. Imagine you make several commits, tweaking the styles or templates until your feature is complete. At this point, you'll be ready to submit your changes back into the shared master branch, which is the branch used for staging deploys. First, make sure your local copy of master is up to date, by switching to it and pressing the sync button. Then click "manage" in the branch menu/tab of your client, drag your feature branch to the first box, and the master branch to the second, then click the merge button. Git will create a new commit that merges your changes into the master branch, taking into account commits that may have been made in the meantime by other people. 

If you and other person changed the same line, you'll have to determine a winner. The conflicted files will be marked in your client, and when you open them in your text editor, you'll see something like this::

<<<<<<<< HEAD
This is the line or lines currently in master.
=====
This is the conflicting line you changed in your branch.
>>>>>>>> LONG_ANNOYING_COMMIT_HASH_STRING

Using your editor, choose which change you want to keep by erasing the others (as well as the conflict markers, like ``<<<<<<< HEAD``). When all the conflict markers are removed, meaning that you've approved the changes you want to keep, the client will show a button allowing you to commit your merge, and then synchronize it up to GitHub. 