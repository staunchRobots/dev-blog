---
layout: post
title: "Keeping your project healthy"
date: 2012-07-28 14:53
comments: true
categories:
published: false
---

Whether you are running consultant company or doing startup or maybe just playing with some code. It's easy to get caught in the flow of development. With increase of features the amount of technical debt for a project is rising.

Once a period, it's very important to step aside of actual development and make a good cleanup. Good mainainable project is important because of:

  * new developers can pick up project very easily and be productive from day 1
  * if you leave project for some time it will be easy to get back.

Here are some tips that help us to run

### rails-footnotes

  If you are developing in Rails you should know the plugin! It displays footnotes in your application for easy debugging, such as sessions, request parameters, cookies, filter chain, routes, queries, etc.

  Even more, it contains links to open files directly in your editor including your backtrace lines. 
   
  <https://github.com/josevalim/rails-footnotes>
  
### Remove trailing spaces
  Trailing spaces is a minor detail but it adds a level of noise in git logs in development process.
  
#### To clean up whitespaces in all files you can use following script
    
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
  On one of our projects I spend tens of hours working on seed script and sample data, so we can deploy full working service in no time when customer need it.
  Good for demo day!

### Change README file, no matter whatever for
  From my experience there is 2 types of readme files
  Reflect changes in structure of project or testing approach or specific requirements that needed.
  <http://tom.preston-werner.com/2010/08/23/readme-driven-development.html>

### Code Climate - collect metrics

  The best way to reflect on code is to collect various code metrics.

### Extract modules

  Go through all the models and see maybe you can extract similar code from models to modules. This will(hopefully!) not break anything inside but make code more readable.

### Newrelic rpm

  Setup free account to monitor your servers. Newrelic is sending emails when your CPU or memory is over threshold.
  Also by default you can browse page response time and find controllers that spend most of resources.

### Use capistrano to deploy(unless you using heroku)

  Use automated deployment! Either heroku or capistrano.

### features staging server

  Setup multiple staging servers per developer, or per feature




