Deployment with Git
===================

We deploy from source control, instead of uploading files individually, for several reasons:

1. It means that our server is always in a known configuration that can be easily re-created from the repo.
2. It prevents simultaneous editing, where people overwrite each others' changes.
3. It allows us to quickly roll back changes if something goes wrong.
4. It comes with a tremendous amount of infrastructure and best practice we can leverage during build processes and review.

It's not hard to get a Git deploy process running, but it does involve a little groundwork. This guide is a short guide to the process, but we'll be covering it in a little more detail in class.

Setup
-----

First you need to link your server to the GitHub account where your repo is hosted. Follow the instructions on `this page <https://help.github.com/articles/generating-ssh-keys>`_ to generate an SSH key and add it to your Git credentials. 

You'll also need to create the repository on GitHub that will contain your repo. This needs to happen whether you have files already or not. For the purposes of this assignment, I'm going to assume that your repository is in an account named ``scc-student`` and the repo name is ``itc210-theme``, but these are just examples.

Finally, you'll need to initialize your local Git repo. Your repo should be a folder inside your ``wp-content/themes`` directory, since it's a theme and that's where themes go. Creating this repo will depend on your operating system, but I would recommend using the graphical clients that are available from GitHub, since they will let you initialize the repo in any location. It may be easiest to have GitHub initialize a repo with a ``readme.md`` file, and then clone that repo into the ``themes`` directory.

Note that only one person should create the initial repo. Once it's created, everyone else should either clone or fork it, depending on your comfort level (for GitHub beginners, it's probably easiest to clone and then add everyone as a collaborator on that repo).

Once you have your server linked to your account, you can log in via SSH and clone the GitHub repo. Navigate to your ``wp-content/themes`` folder and type the following command to clone it (for the sample username/repo given above)::

  git clone git@github.com:scc-student/itc210-theme.git

This will clone the repo into a folder named ``itc210-theme``. If you want to clone it into a different folder, type that folder name at the end of the command before pressing enter.

Deployment
----------

Now that your server and GitHub are connected, and the repo is set up, you can deploy to the server. This is easy to do. First, you'll make changes locally, commit them, and sync them up to the GitHub repo. Second, on the server, you will log in, navigate to *inside* the theme directory, and type the following command::

  git pull origin master

Piece of cake! This command grabs all changes from the remote repo and fast-forwards your server to match. All changes that are checked in will be replicated on the server. **Do not change files directly**, because this will cause the two to fall out of sync, and you won't be able to easily deploy until we make them match again. You should *only* commit locally and push to GitHub, then pull from there to your staging server.