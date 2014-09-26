Requirements: A Requirements Document
=====================================

1. Introducing Requirements
---------------------------

A requirements document is a staple of many development projects,
because it provides a road map for what developers have, and have not,
agreed to do. Writing a requirements document helps to scope out and
organize work, and it prevents feature creep. If you search for samples
online, you'll find a lot of very technical, legally-worded
requirements. I prefer a more casual approach, in keeping with an Agile
philosophy. Don't try to over-specify anything.

Some Agile teams may decide to skip requirements, and instead start
adding items directly to their task backlog, but in my experience, most
of them still put a document together. Since our project is relatively
well-structured (and has a set deadline) it makes sense for us to take a
more structured approach. We will, however, also take the requirements
from this document and add them as issues on the theme's GitHub repo, as
part of this assignment.

2. Overall goals
----------------

The main goal of a requirements document is to get all stakeholders and
developers onto the same page: What are we building? Why are we building
it? What are the use cases that we're trying to address in this project?
Perhaps more importantly, what are we choosing *not* to do?

After it's written, if anyone is confused about the direction
development is taking or what they should tackle next, the requirements
serve as a common touchstone and reference. That said, requirements are
a living document: if they say that you'll do something, but it turns
out that feature is ill-specified or unrealistic given the time/money
allotted, the requirements should change.

3. Background and technical assumptions
---------------------------------------

At this point in our requirements document, we would typically sketch
out the background of our project. In the case of a website design, we
might talk about what the organization does, or what existing site they
have that has proven unsatisfactory. Our background should provide
context for the upcoming user stories.

We also want to lay out our technical assumptions: what are our
platforms, our minimum browser requirements, and any technologies that
are or aren't allowed. For example, we assume for this class that you're
designing a Wordpress theme. You might also assume that the site should
work on IE9+ and evergreen browsers, or that mobile is or isn't a
concern given your target audience.

4. User stories
---------------

Having laid the groundwork, we now get to the real meat of the
requirements document, which is the user stories. These are short
descriptions of tasks performed by a fictional user, each of which ends
up describing a feature. In some cases, the "user" in question is the
admin, but often it's the external consumer of the website. This sounds
more complicated than it actually is, so here's a few examples of user
stories for a film festival website:

User Story: Film scheduling
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A user wants to get information about which films are playing at the
festival, matched against their own busy schedule. They visit the site,
and on the schedule page they find a listing of all the films sorted by
day and time. Each film listing includes the title, director, actors,
and showtime for each film. Clicking on a film title takes the user to a
detail page, where they can read a short description of the film and see
additional posts that relate to it. Users should also be able to filter
the full listing page by day or genre, so they can quickly find which
films will match their festival visit.

(Notes: this user story gives us a lot of information about what the
typical user wants to do with the site, which features we need to build,
and some clues as to how we need to set up our taxonomy. But it doesn't
overwhelm us with technical details at this stage. The goal is to give
us a direction, while still allowing room to be flexible.)

User story: Q&A Recaps
~~~~~~~~~~~~~~~~~~~~~~

The post-film discussions by directors are one of the most interesting
and popular parts of the festival. We'll want to take these online with
a series of posts that are easy to administer. A recap may include a
description of the discussion, audio or video of the panel, and links to
related material. Once a recap is in the system, it should show up in
the film listing page for that film, so that it's easy to find from the
schedule.

(This story gives us some information on what we need to build on the
backend, as well as how it should appear to a user. Whether we decide to
go with a plugin or a custom post type is up to us, but we know that
either solution must support additional metadata for this story to
succeed.)

User story: newsletter sign-up
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The festival has an existing newsletter published via MailChimp. This
new website should give a user an easy way to sign up for the
newsletter, available from all pages. Users should also have the option
to fill out some additional information, such as what kind of films they
enjoy, their contact info, and whether they would be willing to
volunteer for the festival in the future.

(This story will probably be fulfilled via a plugin, but it mentions
some other data that an out-of-the-box solution may not handle. It also
raises some questions about how we take in and store that additional
data. The requirements document doesn't have to answer those questions,
but it should serve as a starting point when developers start to iron
out the details.)

5. Questions
------------

A requirements document in an Agile process is not an end-all, be-all
answer to all questions. Rather, it may also serve as a place to
document where more research is needed. It makes sense to have a section
where you may leave questions, and then return to them later in the
quarter.

6. Saying no
------------

Finally, it makes sense to set some limits early on for what you will
*not* be doing on this project. That doesn't sound controversial, but
feature creep is insidious, and it's good to put your foot down early
(it keeps the team focused). For example, you might lay out the
following things that you're not going to do as a team on this project:

-  You will not maintain an e-mail list as a part of the Wordpress
   database
-  You will not write a shopping cart, or anything that resembles a
   shopping cart
-  You will not promise any features that require a lot of custom
   JavaScript, unless you've got a JS guru on your team
-  You will not use any plugins that aren't GPL licensed

Don't think of the "not doing" section as standing athwart history
screaming "no." Instead, think of it as a powerful tool for making tough
choices. If, while working on the project, you have to choose between
two equally important, but equally time-consuming tasks, having a set of
guidelines that can be used to disqualify one of them from immediate
consideration is extremely helpful.
