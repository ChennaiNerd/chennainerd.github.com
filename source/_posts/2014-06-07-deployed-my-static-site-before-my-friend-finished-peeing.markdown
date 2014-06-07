---
layout: post
title: "Deployed my static site before my friend finished peeing"
date: 2014-06-07 16:50:50 +0530
comments: true
author: Fizer Khan
categories: ["static site", "hosting", "divshot"]
---

Building and Deploying static sites becomes easier and simpler than never before.
I see lot of people buying servers and setting up `cdn` for deploying
sites which may be company sites, personal sites or blogs. I also see people
deploying the sites in S3 which is quiter easier than setting up the server.

But there is also way to deploy your static sites simpler than above methods.
It is [Divshot](http://www.divshot.com/) Hosting.
I deployed my personal blog before my friend finished peeing.
Lets see how i did it.

### Signup for Divshot

Go to [http://www.divshot.com](http://www.divshot.com) and Signup for the service.

### Install Divshot client tool

    $ npm install -g divshot-cli

### Login to Divshot

    $ divshot login

### Create your app directory and place your static files

    $ mkdir app-name
    $ cd app-name

Lets say you are going to have your static assets inside `public` folder as below

    app-name/
      public/
        css/
          main.css
        js/
          main.js
        index.html
        about.html
        contact.html

### Create divshot configuration file

Once you're in your new application's directory, you can initialize a new Divshot
application by using the `divshot init` command. This will walk you step by step
through some basic configuration options for your app, then create a `divshot.json`
file and provision your new app.

    $ divshot init

It will ask you following information

    name: (app-name) app-name
    root directory: (current) public
    clean urls: (y/n) y
    error page: (error.html)
    Would you like to create a Divshot.io app from this app?: (y/n) y
    Creating app ...
    Success: app-name has been created
    Success: App initiated

### Deploy just like git push

To deploy to the development environment, all you need to do is run:

    $ divshot push

Once your app is deployed successfully,
You can view your app at: [http://development.app-name.divshot.io](http://development.app-name.divshot.io)

You can also deploy in production by

    $ divshot push production

    (or)

    $ divshot promote development production

Once you app is deployed in `production` environment, You can view your app at: [http://app-name.divshot.io](http://app-name.divshot.io)

### Setting your custom domain

Once your application is ready, you can set custom domain configuration in divshot.

    $ divshot domains:add www.myapp.com

You also need to set `CNAME` record with your DNS provider.
Refer [Divshot Documentation](http://docs.divshot.com/) for more details.


Happy Hosting and Have a nice day.
