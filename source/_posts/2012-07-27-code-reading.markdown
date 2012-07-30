---
layout: post
title: "Code Reading"
date: 2012-07-27 18:24
comments: true
categories: ruby
published: false
---

### Remark
         
With this article I want to start a series of 'code reading' posts. I love to read source code, especialy good one. And I think it is important for you as a programmer to read others code. You can pick up some tricks or find a better way to structure code or even find new constructions you weren't aware of. Also checkout code reading series: [ARailsDemo](<http://www.arailsdemo.com/posts>), [Code Safari](<http://rubysource.com/?s=Code+Safari>)
                                   
# Code Reading: Retrospectiva      

## What is it?
  [Retrospectiva](<http://retrospectiva.org/overview>) as it says on the site "open-source project management tool, intended to assist the collaborative aspect of work carried out by agile software development teams" by [Dimitrij Denissenko](<https://github.com/dim>). It came under my radar a long time ago and just recently I had opportunity to read across project.
  
  I like a structure of the code and especially extension bindings. It's very rails 2.x-ish and very clean. Everything has it's own place.
  
## $gems_rake_task
  As a good rails app, Retrospectiva have it's core functionality under <code>/lib</code> directory. The interesting thing in <code>environment.rb</code> is how it get activated:
 
``` ruby
  unless $gems_rake_task
    config.active_record.observers = 
      'user_observer', 'project_observer', 'group_observer', 
      'changeset_observer', 'ticket_observer', 'ticket_change_observer'
  end
  ...
  config.after_initialize do
    RetroEM.load!(config)
    RetroCM.reload!
  end unless $gems_rake_task
```                  
  
  Core doesn't get loaded if rails environment is called via rake task. Appears that there is 2 global variables that allow you do define whether you are in rake or rails environment in your code <code>$gems_rake_task</code>, <code>$rails_rake_task</code>!
  
## seeds.rb
  How are yours <code>db/seeds.rb</code> file looks like? Usually I used to put ordinary Rails code there.

``` ruby
  10.times{|i|
    Account.create(:name => "Person_#{i}", :password => "123123")
  }
```
                                                                  
  Althouth it can and need to be represented as a module! Remember [Good coders code,  great reuse](<http://www.catonmat.net/>). Retrospectiva does follow this rule in <code>db/seeds.rb</code>:
``` ruby
  module Retrospectiva
    class DefaultContent
     ...
     def self.create
       new.tap do |creator|
         creator.create_priorities if Priority.count.zero?
         creator.create_group unless Group.exists?(:name => 'Default')
       end
     end
     ...
    end    
  end

  Retrospectiva::DefaultContent.create
```
   
## changelog.rb

  It is great to have CHANGELOG file in your project, but it's usually a pain to see what changed recently from the last version. Why don't let <b>ruby</b> and <b>git</b> do this job for you? [script/changelog.rb](<https://github.com/dim/retrospectiva/blob/master/script/changelog.rb>) contain a ruby script that does the trick. You just need to write a good commit messages.
  
## retrospectiva\_installer.rb

  Another great script for app installation [script/remove/retrospectiva_installer.rb](<https://github.com/dim/retrospectiva/blob/master/script/remote/retrospectiva_installer.rb>)

## tiny\_git.rb
  For integration with git, Retrospectiva using self written module that you could easily pick up at <code>vendor/plugins/tiny-git</code>.
  
## it\_should\_behave\_like

  I wan't aware of this feature of RSpec before. With <code>share_as</code> and <code>it_should_behave_like</code> you can describe general behaviour of an object and then mix it to your current spec. It's like module/include/extend for specs.

``` ruby 
  share_as :EveryProjectAreaController do
  end
  
  ...
  
  describe BlogCommentsController do
    it_should_behave_like EveryProjectAreaController

    # your other tests
  end
```
  
  [Shared example group(RSpec docs)](<http://relishapp.com/rspec/rspec-core/v/2-0/docs/example-groups/shared-example-group>)                                                                                      
  [Specifying mixins with shared example groups in RSpec-2](<http://blog.davidchelimsky.net/2010/11/07/specifying-mixins-with-shared-example-groups-in-rspec-2/>)     
  
## flash.keep

  Keeps either the entire current flash or a specific flash entry available for the next action. [ApiDock](<http://apidock.com/rails/ActionDispatch/Flash/FlashHash/keep>)
 
## Tips

  A trick for 99% of your projects
``` ruby
  config.frameworks -= [ :active_resource ] 
```
                                  
Well thats all for today.
 
