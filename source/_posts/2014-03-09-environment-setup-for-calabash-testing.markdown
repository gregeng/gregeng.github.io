---
layout: post
title: "Environment Setup for Calabash Testing"
date: 2014-03-10 21:25
comments: true
categories: calabash cucumber ruby
published: true
---

In order to begin writing automated test suites with <a href="http://calaba.sh/" target="_blank">calabash</a>, you will need to set up your dev environment.

This is an exciting and necessary step in achieving a productive scripting work flow. So I thought I would write this guide to lower the barrier to entry and help you get up and running sooner.

Both `calabash-ios` and `calabash-android` were originially written as Ruby libraries. Therefore, I will be showing you how to set it up using Ruby and Mac OSX.

Let's get started:<br>

1.  Install <a href="http://brew.sh/" target="_blank">Homebrew</a> (The missing package manager for OS X)<br><br>
    - From the shell run:<br><br>
      - `ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"`<br><br>
2.  Install <a href="https://rvm.io/" target="_blank">RVM</a> (Ruby Version Manager)<br><br>
    - Currently, I am using the following version: `ruby 2.0.0p247`<br><br>
    - In order to replicate my environment, run this from the shell:<br><br>
      - `rvm install 2.0.0-p247`
      - `rvm --default use 2.0.0-p247`<br><br>
3.  Install <a href="https://developer.apple.com/xcode/" target="_blank">Xcode</a>, the Command Line Tools, and iOS Simulators<br><br>
    - Open up the Mac App Store and search for Xcode.<br><br>
    - Install Xcode from there and login using your Apple Developer login/password.<br><br>
    - Once the application is installed, open it up and download the Command Line Tools and iOS Simulators.<br><br>
      - They should be located within Xcode Preferences -> Downloads<br><br>
        - If for some reason they are not available there, search for them <a href="https://daw.apple.com/cgi-bin/WebObjects/DSAuthWeb.woa/wa/login?&appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&path=%2F%2Fdownloads%2Findex.action#" target="_blank">here</a>.<br><br>
4.  Install Required <a href="http://rubygems.org/" target="_blank">Gems</a> (The Ruby Package Management System)<br><br>
    - From the shell run:<br><br>
      - `gem install 'bundler'`
      - `gem install 'calabash-cucumber'`
      - `gem install 'calabash-android'`
      - `gem install 'pry'`<br><br>
5.  Install the <a href="http://developer.android.com/tools/help/adb.html" target="_blank">Android Debug Bridge</a> (adb)<br>
    - Download the Android <a href="http://developer.android.com/sdk/index.html" target="_blank">SDK</a><br><br>
    - Unzip the file and rename the folder to " _android-sdk_ "<br><br>
    - Nest the folder in any directory you like. I suggest `/Users/YOURUSERNAME/Development`<br><br>
    - Add the folder location to your `$PATH`<br><br>
      - From the shell: `cd` then `open -a TextEdit .bash_profile`<br><br>
      - Add this: `export PATH="/Users/YOURUSERNAME/Development/android-sdk/platform-tools/":$PATH`<br><br>
    - Open a new shell window and enter the command `adb`<br><br>
    - If you see a list of adb help commands, you're all set!<br><br>

6.  Generate an Android <a href="https://github.com/calabash/calabash-android/wiki/Running-Calabash-Android" target="_blank">debug.keystore</a><br><br>

    - If debug.keystore is missing, it be recreated in `ANDROID_HOME` with the following command:<br><br>
      ```
      keytool -genkey -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android -keyalg RSA -keysize 2048 -validity 10000 -dname "CN=Android Debug,O=Android,C=US"
      ```<br><br>
7.  Download <a href="https://cloud.genymotion.com/page/launchpad/download/" target="_blank">Genymotion</a><br><br>
    - This is an alternative Android emulator.<br><br>
8.  Download Oracle <a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">VirtualBox</a><br><br>
    - This works in conjunction with Genymotion to simulate Android devices.<br><br>
9.  Enable the Google Playstore on **all** your virtual device simulators<br><br>
    - If you are not signed in with an account on the Google Playstore on each device, you will not be able to run calabash automated scripts in the simulator.<br><br>
    - Here is an excellent <a href="http://stackoverflow.com/questions/17831990/how-do-you-install-google-frameworks-play-accounts-etc-on-a-genymotion-virtu" target="_blank">guide</a> from Stack Overflow detailing how to do this.<br><br>
    - I consistently use the Galaxy Nexus 4.3 API 18 when authoring scripts locally. Of course, feel free to choose any device or version level you want.<br><br>

...and that's it!<br>
Assuming everything went well, you are now ready to begin writing calabash powered, automated test suites. Enjoy!