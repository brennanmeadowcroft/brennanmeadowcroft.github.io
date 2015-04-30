---
layout: post_detail
title:  "ExpressJS Resource Generator"
brief: "Yeoman sped up my development time 20x!"
image: "posts/yeoman.jpg"
date:   2015-04-28 23:19:00
tags: [nodejs, yeoman, expressjs, props2me-experiments]
---

For the last two weeks, I've been hard at work on an iPhone app that provides walking tours in various locales (stay tuned for more info!).  While I await feedback on the comps I provided to my partners, the web services that power the admin interface and feed into the app has been the main focus.

To do this, I decided to use ExpressJS largely because of my experience from the Props2Me Experiments.  It was fresh in mind, had support across a variety of web hosts and didn't have a significant performance hit when compared to some other frameworks considered such as Flask and Rails.

I'm putting CRUD around most of my data models so that I can have full access to them in an administrative interface.  Because of the complexity of my data model, it actually requires a bunch of resources and when you have a model, controller and spec file for each resource, the number of files necessary quickly balloons and even copying and pasting gets tedious.  Each new resource took about ten minutes to complete from start to finish.

This was untenable.  To fix it, I wrote a Yeoman generator that will take the resource name and generate the three files.  Now, I can run ```yo express-generator```, answer the questions and my resource is available.  With the new generator, adding a new resource is about one minute a piece.  In fact, after creating the resource, I can run my unit tests and they all pass.  Before you say anything, the tests failed when I had bugs in my generator so the tests do work as they should.

The experience creating the generator was extremely pleasant and I'm planning on enhancing this one as the need arises.