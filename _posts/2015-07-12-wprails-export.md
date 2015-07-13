---
layout: post
title:  "WordPress to Rails - Environment Set-up and WP Data Export"
date:   2015-07-12 22:34:39
categories: wp2rails
comments: true
---
The first steps to work on the conversion process is to set up our environment and export the data we need.

### Environment

We will be using Rails, Postgresql for out database, Git for version control, and RSpec and Cucumber for testing.

### New Rails App

First to generate a new rails app with git and PostgreSQL, we type this: 

```
$ mkdir longerich_blog_2
$ cd longerich_blog_2
$ git init
$ rails new . --git -T -d postgresql`
```

. creates the app in the current directory.
--git specifies I want to ues git for version control.
-T skips install of Test Unit
-d postgresql uses postgresql instead of SQLite

Next we add RSpec and Cucumber. In our Gemfile we will add the following to our development and test groups.

```
0  group :development, :test do
1   gem 'rspec-rails'
2   gem 'cucumber-rails' :require => false
3   gem 'database-cleaner'
4   gem 'webrat'
7 end
```

Next we run `bundle install` and our environment is set up.

### Export Data

In WordPress there is a built in export feature under Tools > Export. Clicking on it allows you to export posts, pages, comments, custom fields, categories, and tags.

We will be going through this file in the next post to figure out how to export the data into a form that is usable by Active Record.
