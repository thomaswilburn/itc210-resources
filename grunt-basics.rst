Grunt Basics
============

Grunt is a NodeJS build system used by a lot of web projects, since it's portable across operating systems, fast, and effective for a lot of web workflows. It can make your development significantly faster by giving you `LESS <http://lesscss.org>`_ compilation, templating, and live reload.

For this tutorial, I'm assuming that you have a working local WordPress installation, and that you've already downloaded and installed `NodeJS <http://nodejs.org>`_. That should be all you need to get started. You'll also need a shell, but luckily one of those comes with GitHub for Windows and Mac. Just open up your repo in the client, and choose either "Open in Terminal" (Mac) or "Open in Git Shell" (Windows) from the menu.

Installing Grunt
----------------

Getting Grunt installed is a two-step process. That's because it comes in two parts: the command itself, and the actual task runner. First, we'll get the ``grunt`` command installed. Open your command line inside your repo folder, and run the following command::

  npm install grunt-cli -g

This asks the Node Package Manager (NPM) to install the Grunt command line interface, and to do so globally, so that it's available anywhere on your system. **Note:** on Macs or Linux, you may need to type ``sudo`` in front of the ``npm install`` command if it needs admin access.

Once the command is installed, we need to set up the actual Grunt system. Each project gets its own Grunt setup, which is installed in the ``node_modules`` folder. Installing this is also easy::

  npm install grunt

The ``node_modules`` folder will contain Grunt and any other Node packages that you might install for this particular project. I'd recommend that you not check these in, however. We'll tell Git to ignore ``node_modules`` by adding it to the ``.gitignore`` file. You can do that manually, or use the following command to tack it onto the file::

  echo "node_modules" >> .gitignore

We're also going to install a couple of Grunt plugins to do LESS compilation and file watching using NPM::

  npm install grunt-contrib-less grunt-contrib-watch

Your First Gruntfile
--------------------

Now that we have it installed, we can tell Grunt about the tasks we want it to automate. You can run any NodeJS scripts you want from Grunt, but its biggest strength is that it can be simply configured to run plugins. In this case, we're going to enable LESS compilation and live reload using a file watch, so that simply saving the file will cause your browser to reload with the changes.

Create a new file in your project folder named ``Gruntfile.js`` and insert the following code::

  module.exports = function(grunt) {
    //load our plugins that were installed via NPM
    grunt.loadNpmTasks("grunt-contrib-less");
    grunt.loadNpmTasks("grunt-contrib-watch");

    //set our options
    grunt.initConfig({
      less: {
        dev: {
          //compile from style.less to style.css
          src: "style.less",
          dest: "style.css"
        }
      },
      watch: {
        options: {
          livereload: true
        },
        //when .less files change, run the LESS task and livereload
        less: {
          files: ["**/*.less"],
          tasks: ["less"]
        }
      }
    });

    //default task: compile LESS and start the file watch
    grunt.registerTask("default", ["less", "watch"]);

  }

You should also create a ``style.less`` file for our task to compile. Here's an easy one, using a variable and some nesting::

  @brand-color: darken(red, 50%);

  body {
    margin: 0;

    h1 {
      color: @brand-color;
    }
  }

Now we start up our build system from the command line by simply typing ``grunt`` and pressing return. The default task will run, and you should see the following output::

  $ grunt
  Running "less:dev" (less) task
  File style.css created: 0 B → 127 B

  Running "watch" task
  Waiting...

The watch task is now keeping an eye on your files, and will notice when you save a new version, If you update your ``style.less`` file, you'll see it notice the change and re-run the LESS task to produce a new ``style.css`` file (which is what you should actually load in your template).

Finally, let's enable live reload. The documentation for `watch <https://github.com/gruntjs/grunt-contrib-watch/blob/master/docs/watch-examples.md#enabling-live-reload-in-your-html>`_ says to add the following script tag to our page to turn it on::

  <script src="//localhost:35729/livereload.js"></script>

Once that's enabled, you'll see the page refresh on its own whenever the watch task runs. That means you don't need to switch back to your browser, ever: just keep it on one side of the screen, with your editor on the other, and it'll reload whenever you save a file that's being watched. Speaking personally, this is a huge productivity boost.

Where you go from here
----------------------

LESS and watch are great starting tasks, but Grunt can do much, much more. Take a look at its `plugin library <http://gruntjs.com/plugins>`_ to see many of the other capabilities Grunt can offer out of the box, including:

* Compile and concatenate JavaScript into a single file for faster loading on mobile
* Generate responsive images
* Create a spritesheet from separate icon files
* Simplify your Modernizr settings
* Minify your images
* Generate templates from languages like Jade and HAML
* Inject custom banners and other text into all .php files
* Check your PHP and JavaScript for code errors and style issues
* Run automated tests against your pages to find bugs

By using a tool like Grunt, it's possible to automate any of the repetitive tasks that you might otherwise find yourself doing over and over again. It also keeps processes uniform and enforces commonality: people may deviate from the plan, but Grunt will always do the same thing. As a standard industry practice, even if you don't end up using Grunt, you should be aware of it and prepared to use it when contributing to open-source projects.