---
layout: post
title: "Up and Running With AngularJS &amp; Rails"
author: "Ulrich Soeffing"
date: 2013-10-31 19:41
comments: true
categories: 
---
Lately, we at StaunchRobots have started working with AngularJS; a new and increasingly popular Javascript Framework. The purpose of this post is not to describe the great things AngularJS provides you but to outline what we have found to be the best way to start to develop with AngularJS and Rails.

AngularJS is a client-side framework and therefore talks to backend services such as a JSON API. The backend can be programmed with whatever you prefer on the server-side such as Node, Rails or Django.

For the development of the AngularJS app we use [Yeoman](http://yeoman.io/). Yeoman is a collection of tools that improves and speeds up your development workflow. You can find instructions on how to install Yeoman on their [website](http://yeoman.io/).

So enough talk, let’s get this thing rolling.

1. Create the Rails & Angular App

```bash
rails new angular_rails
cd angular_rails
```

Create the AngularJS app within the root directory of the rails project. To do this you will need to install the angular generators that ship with Yo, one of the tools found within Yeoman.

```bash
mkdir angular_client && cd $_
npm install -g generator-angular  (in case you have not installed them)
yo angular client
```

The last command creates a project skeleton for the AngularJS client app and installs the necessary node packages. For a full list of yo angular generators have a look [here](https://github.com/yeoman/generator-angular).

From now on you should have two terminal windows open. Once parked in the rails root directory from where you run rails specific tasks (rake db:create, rails server, etc.) and the other within the ‘client’ directory inside the rails root directory from where you run AngularJS specific tasks.

Now, while developing the app you will have two servers running. The rails server which provides the JSON API and the development server for your AngularJS client started via the command ‘grunt server’. In order to proxy requests made from the angular app to the rails server you will need to modify the Gruntfile.

3. Modify the Gruntfile rails_root_path/client/Gruntfile

First, add the following line to the Gruntfile: 

```
var proxySnippet = require('grunt-connect-proxy/lib/utils').proxyRequest;
```
And specify your proxy setting:

```
proxies: [{
  context: '/api/v1',
  host: 'localhost',
  port: 3000,
  https: false,
  changeOrigin: false
}]
```

Then, configure the folder to which the ‘grunt build’ command will copy and compile the angular client app just before deployment. Currently we put the angular client inside the ‘public’ folder within the rails root directory and have it served as a static files.

```
var yeomanConfig = {
  app: 'app',
  dist: '../public'
};
```

You can see a complete gist of a Gruntfile here:

<https://gist.github.com/soeffing/6434784/>

Finally, run the following command which will install the necessary node package to proxy requests from the angular client to the rails backend.

```bash
npm install grunt-connect-proxy --save-dev
```

4. Getting around CORS issues

Now fire up the two servers (rails server & grunt server). When you try to communicate with the rails API via the http or ngResource services provided by AngularJS you will run into [CORS](http://en.wikipedia.org/wiki/Cross-%20origin_resource_sharing) issues. This occurs because the two servers run on different domains (localhost:3000 (rails) & localhost:9000 (angular)). In order to allow CORS add the following code to your application_controller.rb file:

```ruby

before_filter :cors_set_access_control_headers

def cors_set_access_control_headers
  headers['Access-Control-Allow-Origin'] = '*'# need to be changed once it goes to production 'http://localhost:8080'
  headers['Access-Control-Allow-Methods'] = 'POST, GET, PUT, DELETE, OPTIONS'
  headers['Access-Control-Allow-Headers'] = '*, X-Requested-With, X-Prototype-Version, X-CSRF-Token, Content-Type'
  headers['Access-Control-Max-Age'] = "1728000"
end
```
Now you are clear to develop with AngularJS and Rails. Before deployment you have to run the ‘grunt build’ command within the ‘client’ directory which will compile and copy the angular client app into the public folder of the Rails project.

Expect more AngularJS related posts to come.

Happy coding!