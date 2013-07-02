---
layout: post
title: "Sinatra app to deploy master branch based on SemaphoreApp payload"
description: ""
category: 
tags: [campfire semaphoreapp passenger deployment deploy build sinatra json apache ruby]
---
{% include JB/setup %}

At work we use SemaphoreApp to run our test builds for a reasonably sizable
Ruby on Rails app on a regular basis.  We've configured the builds to send a
payload of JSON data upon completion of each build to a little Sinatra app I
developed.

The app automatically accepts the data from SemaphoreApp and deploys our master
branch to a staging server if the build passes, which has been extremely helpful
in streamlining our QA process.

So let's see how this app is setup...

This Sinatra app is installed on our staging server as follows:

$ **pwd**

    /var/www/deploy

Here's our project tree and the associated files' source code:

$ **tree**

    .
    |-- Gemfile
    |-- Gemfile.lock
    |-- app.rb
    |-- config.ru
    |-- public
    |-- tmp


# Gemfile

    source :rubygems

    gem 'shotgun'
    gem 'sinatra'
    gem 'yajl-ruby'
    gem 'tinder'


# app.rb

    post '/deploy' do

      json = request.body.read
      data = Yajl::Parser.parse(json)
      token = ENV['CAMPFIRE_TOKEN']
      subdomain = ENV['CAMPFIRE_SUBDOMAIN']
      chatroom = ENV['CAMPFIRE_CHATROOM']
      campfire = Tinder::Campfire.new subdomain, :token => token, :ssl_verify => false
      room = campfire.find_room_by_name(chatroom)
      commit = data['commit']
     
       if data['branch_name'] == 'master' && data['result'] == 'passed'
         room.paste <<-END_OF_MESSAGE
           Deploying to staging
           Commit SHA1: #{commit['id']}
           Author: #{commit['author_name']}
           #{commit['message']}"
         END_OF_MESSAGE

         command = "cd ~/path/to/deployment; git pull origin master; cap deploy staging"
        pipe = IO.popen(command)
        response = pipe.readlines
      else
        room.paste <<-END_OF_MESSAGE
          Skipping SemaphoreApp payload
          Branch: #{data['branch_name']} 
          Build status: #{data['result']}
        END_OF_MESSAGE
      end
    end


# config.ru

    require 'rubygems'
    require 'sinatra'
    require 'yajl'
    require 'tinder'
    require File.dirname(__FILE__) + "/app.rb"
    run Sinatra::Application


# /etc/httpd/conf/httpd.conf

    <VirtualHost *:80>
      ServerName your.ip.address
      DocumentRoot /var/www/deploy/public
      <Directory /var/www/deploy/public>
        AllowOverride all
        Options -MultiViews
      </Directory>
    </VirtualHost>
