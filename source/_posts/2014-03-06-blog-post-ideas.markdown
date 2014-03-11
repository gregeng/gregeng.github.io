---
layout: post
title: "blog post ideas"
date: 2014-03-06 20:29
comments: true
categories: ruby rails heroku bdd testing qa cucumber calabash non-technical
published: false
---

## some of this is for you but most of it is for me.. alot of this is to help me remember what i learned and serve as a reference to remind myself things later ##

### 1. non-technical explanation post ###

categories: QA, calabash, cucumber, ruby, mobile, android, ios, non-technical, testing,

Hello Again

Hello Again - Calabash

It's been a few months since I've updated my blog and I apologize! A lot has happened since then. I graduated from the Flatiron School, moved from New York to San Francisco, and started a new job.

For the past 2 months, I've been interning at Xamarin as a Mobile Test Automation Intern.

This means I get to I author **automated** UI test scripts for various Android and iOS applications. These scripts then aid in ensuring the highest quality apps by testing them for functional bugs and UI deficiencies across various devices in a burgeoning product called the Xamarin Test Cloud.

In a previous role, I was **manually** QA testing mobile applications on both platforms, so in a way things have come full circle. It's really exciting to apply new skills to a familiar craft.

In the past 8 weeks I have learned so much and I anticipate learning even more. I want to seize the opportunity to write about it. I'm looking forward to covering topics such as testing, QA, mobile, cucumber, and calabash.

I'm excited to tell you all about it. Until then!

### 2. What is Calabash?

So what is a calabash anyway? Well, its a green gourd... which makes sense because its derived from cucumber.

But in all seriousness, it is a tsting framework automation engined.

BLOG POSTS And VIDEOS

Why don't I let the co-creator of Calabash describe it to you? [Insert Video of Karl Explaining Here]

If you are in a hurry and don't have time to watch the video, here's a quick summary for you:

Android
-server runs along side the app
-resigned
-never rooted
-passes same standards as the Google Playstore

iOS
-server built into the app
-resigned
-never jailbroken
-passes same standards as App Store

explanation of how calabash works, with the server running in parallel versus baked into app ###
-calabash is the automation engine. cucumber is the vehicle. at the end of the day calabash converts the queries into something that literally changes to touch coordinates of where it has determined an element to exist.

### 3. how to start writing android ui tests for any app###
- setting up the environment
-why am i choosing android? because of the way calabash works. the .ipa has to have the calabash framework built in when compiled. with android, you just have to resign it and you're good to go. it runs in parallel to the app.

------- for another blog post
    - download an app from the google play store
    - make a folder named calabash-tests ... mk a folder for the app name
    - adb shell. cd data/app/ ... this is where applications live
    - adb pull .. make an original app folder
    - calabash-android resign
    - calabash-android console
    - reinstall_apps
    - start_test_server

https://docs.google.com/a/xamarin.com/document/d/1bAwXH9sRt7pSii1WTlLMQu5gv50gw0IZWO-zYiVhKvs/edit#

test on a local device so much better.

http://howto.cnet.com/8301-11310_39-57610905-285/how-to-record-your-screen-on-android-4.4-kitkat/

### 4. the basics
- query "*"
- find an element on the page
- query the index of that item
- touch that item
- it all boils down to querying objects. and then touching the objects.
- its as easy as switching the query method to the touch method

### 5. strong and weak bindings.
- query *
- query index number
- query text contains --- localized
- query content description
- query id --- developer buy in

### 6. helpful performAction arguments in calabash android ###
scroll_down
scroll_up
send_key_enter
rotate devicie
swipe
touch and hold
drag coordinates

### 7. granular steps in calabash android and cucumber. takes a screenshot at each step .. helpful for identifying ui bugs. ###
- look up to make sure this is a calabash thing not a cucumber thing
- A A A , Arange, Act, Assert. Given When Then



### 8. the page object patterm/model##
- you have to use an instance variable in the step definition to increase its scope since it is given within the step definition. In a future post, I will outline how this pattern fits in in terms of moving up the abstract ladder with calabash

## 9. a tale of 3 logins##
- good, better, even better (I dont like to throw the word best around lightly.)

## 10. .b.pdd##
-binding.pry driven development - the mobile automation workflow
Step 1
Launch the app from a local device or simulator
use `rake console` or calabash-android path_to_apk
within the irb console `reinstall_apps`
within the irb console `start_test_server_in_background`
navigate through various screens in the app and query different objects to get a feel for what you can work with
begin thinking of the flow for a use case or feature
think in terms of possible page objects you could construct and their methods
Pages
LoginPage, TourPage, HomePage
Methods
take_tour, enter_credentials, login
Most of the time the first feature you build will be a valid login

now it doesnt quite matter what the enter credentials method entails. i know that i wrote it at some point and that it works. the details are only a concern of past greg. all future greg needs to know is that the login page has a method called enter credentials. you can pass a login and a password and be on your merry valid authorized user way.

Step 2
Create a new feature file under app_name/features
Use convention ##_feature_name.feature
Using gherkin, write out in plain english the feature as you expect it to run

Step 3
Run the feature using `rake run` from the command line, which is a shortcut for
calabash-android run `path_to_apk`
Calabash will output to the console pending step definitions

Step 4
Create a new step_definition file under app_name/features/step_definitions
use convention feature_name_steps.rb
Copy and paste the pending step definitions from the command line to this file

Step 5
Based on what you discovered in the first step, begin to fill in your step definitions with the page objects and methods you will probably use.
@current_page is the instance variable you assign after instantiating a page object to expose the methods available at the time.
`page` is a helper method that instantiates each PageObject and .await ensures the code waits for the screen you intended before calling methods.

Step 6
Page objects are conventionally written in camel case, while their filename counterparts are in snake case
Create pages for everything you outlined in the step definitions under android/pages
convention page_name_page.rb
Under each page class, define a distinguishing trait as a method. The method takes in a query string. The trait should be something unique to that page.
Create methods for all other interactions you want to gain from the pages. It will be logical based on the way you wrote the step definitions.

Step 7
Run the tests using `rake run`
After confirming it work locally, upload to XTC

Pro-Tips
Use the pry gem to insert breakpoints into your app. When you run the scripts it will load all your page objects and methods allowing you to interact with the world your test-suite is defining
Abstract the entire login feature into a single step called `Given I am logged in` and put it under a step definition file called common_steps.rb


### 11. creating a custom scroll in calabash-android ###
- use frame layout. this always shows the size of the screen.
- get the xy coordinates
-That thing I showed him .class ? .keys .first .class .first etc
- helpful ruby functions.
-multiply by each to determine scroll speed.


### 12. random. type writer print ###
-shout out to my friend david bella. would like to incorporate this into ui tests in calabash-android for showman purposes.

### 13. random helpful adb tools, view hierarchy ###

#Important Docs to Adapt to Blog Posts#
-Technical Training
-Git Resources
-Environment Setup
-That thing I showed him .class ? .keys .firt .class .first etc