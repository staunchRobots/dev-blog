---
layout: post
author: "Vladimir Penkin"
title: "Keeping your project healthy"
date: 2012-10-17 14:53
comments: true
categories:
---

Whether you are running consultant company or startup or maybe just playing with some code. It's easy to get caught in the flow of development. With increasing amount of features the amount of technical debt for a project is rising as well.

Once a period, it's very important to step aside of actual development and make a good cleanup. Good mainainable project is important because of:

  * new developers can pick up project very easily and be productive from day 1
  * if you leave project for some time it will be easy to get back.
                                      
I always find myself to do that kind of cleaning up when I join projects. 

Here are some tips that help me to develop and maintain projects.

### rails-footnotes

  If you write Rails applications on the daily basis, you should know this plugin by Jose Valim. What it do, is displaying all kind of interesting information that happen in the cycle of request. Sessions, request parameters, cookies, filter chain, routes, queries.
             
  There is text editor integration with MacVim, Textmate and SublimeText2. You can jump to particular place in code from footnotes. Also it replaces error page with proper links to files.

  <https://github.com/josevalim/rails-footnotes>

  Also take a look at more light alternative:
  <http://miniprofiler.com/>

### Bullet

  The Bullet gem is designed to help you increase your application’s performance by reducing the number of queries it makes. It will watch your queries while you develop your application and notify you when you should add eager loading (N+1 queries), when you’re using eager loading that isn’t necessary and when you should use counter cache.

  <https://github.com/flyerhzm/bullet>

### Remove trailing spaces

  Seriously, remove them right now! Trailing spaces is a minor detail but it adds a level of noise to git logs in development process. It is very annoying!

![Trailing noise](/images/trailing_whitespaces.png)

#### To clean up whitespaces in all files in project you can use following command

```bash
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

### Keep your seeds fresh!

  Seeds are very important, whether it's demo(developer) data or actual production seed. 
  When updating models associations or structure do not forget to reflect this changes in seeds.rb file.
  On one of our projects I spend TENS of hours working on seed script and sample data, so we can deploy fully working staging application in no time. Any time that customer wants to check a progress. Helfull in a demo day.

  Another antipattern that I usually see is keeping your seed data in migrations. Seriously why you want to digg to migration every time you forget password for admin user?

[active_admin example](https://github.com/gregbell/active_admin/blob/master/lib/generators/active_admin/devise/devise_generator.rb#L49)

### Extract modules

  Go through all the models and check if you can extract some code from models to general utility modules. This will(hopefully!) not break anything inside but make code more readable.
  Also consider to going even further - apply SRP(Single Responsibility Principle) and compose classes that do only one thing, but do it good.

### Change README file, no matter whatever for
  There is 3 types of readme files for rails projects. Most popular is almost empty readme file with project name and short description. Next by popularity is file starting with line "== Welcome to Rails". Rare beast is readme file that actually reflects the project, shows you <code>installation instructions</code>, <code>usage</code> patterns, goes through custom <code>rake tasks</code> that used to run project and <code>guide to run tests!</code>
  
Check out [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html)

### Do not forget about /doc folder

  This folder can be very good to keep your project documentation. Of course we all know about github wiki pages or basecamp or any other knowledge base of choice. But do you really want to be dependent of external service to keep your documentation?
  
  With just a few keystrokes in your editor you can have access to all documentation related to project. Credentials, setting up development machine, deployment, testing, tips, technical debts, just random ideas, list can go forever.
  
  But wait, it's not the best thing yet. The best thing is you can __track__ changes in documentation just going through commits! Why use two places for tracking news in the code instead of one?(SCM)          
  
```bash
  git rm -r --cached doc/
  git commit -m 'Removed doc folder from repo'
  rm -rf doc/
  
  git submodule add git@github.com:user/project.wiki.git doc
  git commit -m 'Initialize wiki doc repository'
```
                                        
Before running that script you need to tell github to create wiki repository by hitting url: <code>http://github.com/user/repo/wiki/_access</code>
Your doc/ folder will be separate submodule inside your repo. You can keep a good track of what is happening inside doc/ directory.

[Pro Tip](http://coderwall.com/p/vuy3nq)

### RVMRCFY!
  Create and check in in to repository .rvmrc file with specific vestion of ruby that you all be running. This will keep everyone on the same page while development process. I was in situations when my version of ruby as miserably failing on tests while it was working for everyone else.
  
  If you using zsh, here is function that will help you to easily create .rvmrc file for project

[Pro Tip](http://coderwall.com/p/41he7a?i=2&p=1&q=author%3Ashell&t%5B%5D=shell)
                                
### Newrelic rpm

  Setup free account to monitor your servers. Newrelic is sending emails when your CPU or memory is over threshold.
  Also by default you can browse page response time and find controllers that spend most of resources.

[NewRelic](http://newrelic.com/)

### Live metrics

  Instrumental App makes it easy to see what's happening in your application right now.
  Collecting live information about performance of application can get your eyes open on a lot of things. With just few changes you can save hundreds of dollars on hardware.

  Check out [https://instrumentalapp.com](https://instrumentalapp.com)

### Code Climate - collect metrics

  The best way to reflect on code is to collect various code metrics.

  Check out <https://codeclimate.com/>

### Use capistrano to deploy(unless you using heroku)

  Use automated deployment with no excuse! Either heroku or capistrano or even rsync

  [gist](https://gist.github.com/2161449)

### Wrapping up

  This is list of most common things in every project, hope this will help you to make your project more maintainable. <3 <3 <3