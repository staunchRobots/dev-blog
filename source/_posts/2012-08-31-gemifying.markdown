---
layout: post
author: "Jeffrey Biles"
title: "Gemifying your app for fun and profit"
date: 2012-08-31 14:53
comments: true
categories: ruby
---

So you want to make a gem?  Here's how:

##First, make something you'd like to gemify

I'm not going to help you with this part.  For now let's just assume that you have an application that you'd like to share with the world in an easy way.  If you're working in the ruby world, that's through creating a gem.

###Wait, tell me again why we're doing this...

Let's use my specific situation to illustrate.  We're making a command line application that we share with the subscribers of our service so that if they are versed in *nix systems they can automate the use of our service.  Before I gemified the command line application, here's what a user had to do:

1. Clone the files off github
2. cd into the new directory
3. bundle install
4. set up user credentials in a yaml file
5. type "bin/cloudalign"

In addition to all of these steps, step 5 will ONLY work in one directory.  Let me show you the steps after gemifying:

1. gem install cloudalign-cli
2. set up user credentials in a yaml file
3. type "cloudalign"

Not only are there fewer steps, but two of the three steps that are left require less technical know-how (and we're working on making step 2 easier as well, but simply gemifying doesn't help that).

If you're convinced, let's go.  If you're not, then go ahead and distribute your app the old-fashioned way and come back when you're wiser ;)

##First, make a gemspec

In the main directory, create a file called YOUR_APP_NAME.gemspec.  In this file, you'll put something like this:

	Gem::Specification.new do |s|
	  s.name = 'cloudalign-cli'
	  s.version = '0.1.1'
	  s.platform = Gem::Platform::RUBY
	  s.authors = ['Jonathan McCaffrey', 'Jeffrey Biles']
	  s.summary = %q{Access cloudalign from your command line}
	  s.description = %q{Cloudalign brings the power of the cloud straight to your lab}
	  s.rubyforge_project = "cloudalign-cli"
	  s.files = Dir.glob("{bin,lib}/**/*") + %w(README.md)
	  s.executables = ["cloudalign"]
	  s.homepage = 'http://rubygems.org/gems/cloudalign-cli'

	  s.add_dependency("thor")
	  s.add_dependency("rest-client")
	  s.add_dependency("formatador")
	  s.add_dependency("rspec")
	end

Most of these fields are self-explanatory.  Name, authors, summary are exactly what they sound like.  add_dependency is just a list of the other gems required (what you have in your gemfile).  The version number is for semantic versioning (or, if you prefer, terrible non-semantic versioning).

The ones you must pay attention to are 'files' and 'executables'.  The 'files' entry was stolen from (if memory serves correctly) a blog post by yehuda katz.  You can copy it verbatim, though you may want to include a few more extraneous files aside from the readme.  In the 'executables' entry, you put the main executable file of your app, usually found in the 'bin' folder.

###Second, bundle it all up

Like the previous steps, this one is also easy.  In the command line, while in the main directory of your app, type `gem build YOUR_GEMSPEC_FILE`.  You'll get a message saying that a gem has been created.  This will be a file named `GEM_NAME-GEM_VERSION.gem`.

Test this by typing `gem install GEM_NAME-GEM_VERSION.gem` (installing using the gem that was just created).  This installs it in your system, as a gem.  Finally, test it by running it just like you expect your users to.  In this case, I would type `cloudalign project list`, and sure enough it works as expected (return a list of the projects that the user is involved in).

Great!  You've got a gem!  Now to distribute it.

###Third, push to rubygems

Rubygems is the standard way that developers publish their open-source gems.  If you wish to keep your code closed-source, there are [ways to do that](http://www.cerebris.com/blog/2011/03/15/creating-and-managing-private-rubygems-with-jeweler-github-and-bundler/), but hosting your gems on rubygems is the general practice, and extremely simple to boot.

Here are the steps

1. [sign up for a rubygems account](https://rubygems.org/users/new)
2. download some of their tools by typing `curl -u RUBYGEMS_USERNAME https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials`
3. type `gem push GEM_NAME-GEM_VERSION.gem`

###Fourth, test on another project

In another project, type `gem install GEM_NAME`.  Test to see if it works as intended.

###Fifth, fun

Your code is now WAY easier and more fun for others to use.

###Sixth, profit

(unless you're twitter)
