---
layout: post
author: "Vladimir Penkin"
title: "Keeping your project healthy"
date: 2012-07-28 14:53
comments: true
categories:
---

Whether you are running consultant company or startup or maybe just playing with some code. It's easy to get caught in the flow of development. With increase of features the amount of technical debt for a project is rising.

Once a period, it's very important to step aside of actual development and make a good cleanup. Good mainainable project is important because of:

  * new developers can pick up project very easily and be productive from day 1
  * if you leave project for some time it will be easy to get back.

Here are some tips that help us to run

## Development / Code

### rails-footnotes

  If you are developing in Rails you should know the plugin! It displays footnotes in your application for easy debugging, such as sessions, request parameters, cookies, filter chain, routes, queries.

  Even more, it contains links to open files directly in your editor including your backtrace lines.

  <https://github.com/josevalim/rails-footnotes>

  Alternative solution:
  <http://miniprofiler.com/>

### Bullet

  The Bullet gem is designed to help you increase your application’s performance by reducing the number of queries it makes. It will watch your queries while you develop your application and notify you when you should add eager loading (N+1 queries), when you’re using eager loading that isn’t necessary and when you should use counter cache.

  <https://github.com/flyerhzm/bullet>

### Remove trailing spaces

  Trailing spaces is a minor detail but it adds a level of noise in git logs in development process. It is really annoying

#### To clean up whitespaces in all files in project you can use following command

```
    find . -not \( -name .svn -prune -o -name .git -prune \) -type f -print0 | xargs -0 file -In | grep -v binary | cut -d ":" -f1 | xargs -0 sed -i '' -E "s/[[:space:]]*$//"
```

  Script is going through files all except binary and source control files and removes trailing whitespaces.

#### Setup text editor to remove trailing spaces before saving file

  * _sublime_

    Recently popular Sublime Text 2 have this functionality built in by default. You just need to add following string to configuration file:
```
  "trim_trailing_white_space_on_save": true
```

  * _textmate_

    Here is textmate bundle to Convert Tabs To Spaces and Remove Trailing Whitespace

    <https://github.com/glennr/uber-glory-tmbundle>

  * _vim_

    If you are using vim, it's a very good chance you already setup this functionality.
    If not, you can use this link <http://rails-bestpractices.com/posts/60-remove-trailing-whitespace>

### Keep Seed data in seeds! Seed is very important for everyone

  On one of our projects I spend tens of hours working on seed script and sample data, so we can deploy full working site in no time when customer need to see it.
  Good for demo day!

### Extract modules

  Go through all the models and see maybe you can extract similar code from models to general utility modules. This will(hopefully!) not break anything inside but make code more readable.
  Also consider to going even far with SRP(Single Responsibility Principle) and compose classes that do only one thing, but do it good.

### Change README file, no matter whatever for
  From my experience there is 2 types of readme files
  Reflect changes in structure of project or testing approach or specific requirements that needed.
  <http://tom.preston-werner.com/2010/08/23/readme-driven-development.html>

### Do not forget about /doc folder

  This folder can be very good to keep your project documentation. Of course we all know about github wiki pages or basecamp or any other knowledge base of choice. But do you really want to be dependent of external service to keep your documentation?
  With just a few keystrokes in your editor you can have access to all documentation related to project. Credentials, setting up development machine, deployment, testing, tips, technical debts, just random ideas, list can go forever.
  But wait, it's not the best thing yet. The best thing is you can __track__ changes in documentation just going through commits! Why use two places for tracking news in the code instead of one?(SCM)


## Metrics, metrics, metrics

### Newrelic rpm

  Setup free account to monitor your servers. Newrelic is sending emails when your CPU or memory is over threshold.
  Also by default you can browse page response time and find controllers that spend most of resources.

### Live metrics

  Instrumental makes it easy to see what's happening in your application right now.
  Collecting live information about performance of application can get your eyes open on a lot of things. With just few changes you can save hundreds of dollars on hardware.

  Check out <https://instrumentalapp.com>

### Code Climate - collect metrics

  The best way to reflect on code is to collect various code metrics.

  Check out <https://codeclimate.com/>


## Server / Deployment

### Use capistrano to deploy(unless you using heroku)

  Use automated deployment with no excuse! Either heroku or capistrano or even rsync
  https://gist.github.com/2161449

### feature and staging server

  Setup multiple staging servers per developer, or per feature
