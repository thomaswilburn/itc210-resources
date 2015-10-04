ITC 210 - Fall 2015
===================

Summary
-------

This is the Fall 2015 syllabus for ITC 210 at Seattle Central College. In this class, students will be partnered with designers to build a site on WordPress for an external client, using real-world tools and techniques. The resulting WordPress theme will be a useful portfolio piece (we hope).

Required Materials
------------------

There is no textbook for this class. Reading materials will be posted to this repo as needed, either as tutorial texts or links to online resources. For your own part, you will need the following software programs and services:

* A staging server: You will need a virtual private server (VPS) with PHP, MySQL, Git, and SSH access. Do not use so-called "shared hosting", the performance will be sub-par and you will not be able to deploy easily. Dreamhost offers VPS service for $15 a month, but I recommend looking into Linode ($10/month), Digital Ocean ($5/month), or Amazon EC2 (free for a year, varies after that). Again, *do not use shared hosting*.
* A GitHub account: your WordPress theme must be hosted on GitHub, and preferably will be deployed directly from there to your staging server. All members of your team should collaborate on the same repository. You should also have Git/GitHub installed on your development machine.
* A local PHP/MySQL environment for development. Depending on your computer, you may want to use one of the WAMP/MAMP stacks, or create a Linux virtual machine and work there. The goal is to work locally, then check changes to GitHub, and from there to the staging server. Under no circumstances should you be editing files directly on the remote server.
* A syntax-highlighting code editor: luckily, you have many free options here, including Komodo, Brackets, Caret, and Notepad++. You may also want to look at paying for a copy of Sublime Text, which is well worth the money.

Assignments
-----------

For the most part, your team should work in one-week sprints, as with an Agile workflow. As developers, your work will parallel the designers, and although you do not share assignments you should consider their work an extension of yours. Developers who hang their designers out to dry will suffer penalties. Developer-specific assignments are as follows, although I reserve the right to alter or rearrange them at any time. Individual assignments will be posted with more detail throughout the quarter, although a rough list follows. All assignments are due by midnight on Friday.

For each individual task within an assignment, create a bug in the issues section of your theme repo, and make sure to close it with references to the relevant commits. Tracking tasks this way will let you better estimate your time, and it will provide me with important information as to who is doing work. If you don't have commits, as far as I'm concerned, you didn't do any work.

Final grades will be based on these assignments for 70% of their total, 20% for your final presentation, and 10% for class participation. Attending class is not optional.

Week 1 - Server setup
#####################

You should have your staging server up and running, your GitHub repo for your theme stubbed out, and a deployment connection made between the two. The theme only needs to include ``style.css`` and ``index.php`` files for now--you'll be filling in the rest as you go. Email your repo URL and development server address to me via scc@thomaswilburn.net.

*Due: Friday, Oct. 2*

Week 2 - Requirements document
##############################

Created with your designers, this document will lay out the basic outline of intended work for your site, including extra functionality that will be handled with plugins, browser compatibility, and user stories. See the sample `requirements document <https://github.com/thomaswilburn/itc210-resources/blob/master/requirements.rst>`_ (which doubles as a guide to requirements docs) for more information.

*Due: Friday, Oct. 9*

Week 3 - Information architecture
#################################

At this point, you should understand the ontology of your site. With this information, make sure that your theme matches its organization: menus should lead to placeholder pages, and the template heirarchy should reflect the basic structure in which your site is constructed. You are not required to have this in its final state, but I should see evidence of groundwork being laid.

Week 4 - Plugin documentation and configuration
###############################################

As your designers work on wireframes, you will pick out the plugins that you need to achieve their functionality requirements, install them, and write up documentation on the install/config process for your end-user. Extra points for using `Composer <http://getcomposer.org>`_ and `WPackagist <http://wpackagist.org/>`_ to install, instead of doing so manually.

Week 5 - Information architecture 2
###################################

In order to wireframe effectively, you will need to start putting in placeholder content and check to see that your organization holds up. For this sprint, I want to start seeing template partials and sidebars added to the page, with no shared elements being duplicated between templates.

Week 6 - Unstyled theme
#######################

With wireframes complete, you should be able to duplicate their basic arrangement in the browser. Don't worry about colors or exact layout, but the flavor of the page should be there and it should match the mocks on desktop. All basic functionality and navigation should exist.

Week 7 - Responsiveness
#######################

If you started with a decent grid, this work may already be done. At the end of this week, your site should be able to match the wireframes on mobile as well as desktop, in terms of physical arrangement and basic functionality.

Week 8 - Color palette
######################

Update your unstyled theme to match the colors and textures of the visual design. Load any webfonts that may be required. Produce a list of missing assets that your designers need to provide.

Week 9 - Visual match
#####################

Your designers are required to have page templates ready, which means you are too. After this point, your site should be in code freeze: the rest of the time after this should be spent on testing, bug fixes, and deployment.

Week 12 - Presentation
######################

In the last week of class, you will present your project as a group to the rest of the students.

