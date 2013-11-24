---
layout: post
title: "Hide Your Keys, Hide Your Tokens ... Unless Deploying to Heroku"
date: 2013-11-21 09:33
comments: true
categories: ruby rails heroku api gems
published: true
---

In that case, you have to do a little more than just hide them.

Typically, an API will require a key or authentication token before serving data downstream. It's logical to avoid committing any of these to a public repo on GitHub, so I often found myself falling into a pattern. I would make hidden files in the assets folder of my project, then `.gitignore` each one.

It would result in a folder structure that might look like this:
```bash
/project
    /lib
      /assets
        .api_key
        .auth_token
        .password
```

Then in the class that controls all of the API calls, I would do something like this:

```ruby
Class API

  API_KEY = open('lib/assets/.api_key').read()
  AUTH_TOKEN = open('lib/assets/.auth_token').read
  PASSWORD = open('lib/assets/.password').read

  def some_method
    :user_key => API_KEY
    :user_token => AUTH_TOKEN
    :user_password => PASSWORD
  end

end
```
I set each relevant piece to a constant to be used in methods throughout the API interactions.

This way feels a little clunky, but it works. As long as you remember to add each hidden file to the .gitignore, it keeps the credentials safe. But as a project grows and it accumulates more authorizations, it becomes a little difficult to manage.

It wasn't until my group tried to deploy an app to Heroku that we discovered the better way. Heroku makes it simple to deploy a Rails app, but as a result it lacks a lot of the bells and whistles. As a result, it is impossible to log in to the server in a shell to upload each individual hidden file described above.

In order to accomplish the same level of security and function, we can instead use Heroku config variables that will be read at runtime. Heroku has some great <a href="https://devcenter.heroku.com/articles/config-vars#local-setup" target="_blank">documentation</a> on this topic.

It's very easy to set up from the local shell. Thanks to my teammate <a href="http://davidbella.github.io/" target="_blank">David Bella</a>, we were able to figure out what we needed to do immediately.

Just navigate to the root of your project directory and run the `heroku config:set` command.

For example:
```bash
heroku config:set API_KEY=abcefghijklmnopqrstuvwxyz
heroku config:set AUTH_TOKEN=abc123doremi
heroku config:set PASSWORD=p4ssw0rd
```
That's all there is to it! If you want to see your setup at any point, just type `heroku config` to see an output like this:

```bash
API_KEY:                     abcefghijklmnopqrstuvwxyz
AUTH_TOKEN:                  abc123doremi
PASSWORD:                    p4ssw0rd
```
This is great for production, but how can we get the same level of organization and clarity for our development environment? My classmate <a href="http://scottluptowski.com/" target="_blank">Scott Luptowski</a> told us about the <a href="https://github.com/bkeepers/dotenv" target="_blank">dotenv</a> gem that assists with that exact problem.

Start by including `gem 'dotenv-rails'` in your `Gemfile` in the appropriate groups. We only use it in `:test` and `:development`. Then, make a `.env` file at the root of your project directory. In it, include the assignments for each of the keys, tokens or passwords you want to call.

For example:

```bash
API_KEY=abcefghijklmnopqrstuvwxyz
AUTH_TOKEN=abc123doremi
PASSWORD=p4ssw0rd
```
Rails will be able to call these variables anywhere in the application by using this pattern: `ENV['API_KEY']`, `ENV['AUTH_TOKEN']`, `ENV['PASSWORD']`. The hard coded keys will stay out of your code and you will have one organized place to manage all the necessary information. Just remember to also add the .env file to your .gitignore. The app should now work seamlessly in both local development and production Heroku environments. A simple solution for an important problem!
