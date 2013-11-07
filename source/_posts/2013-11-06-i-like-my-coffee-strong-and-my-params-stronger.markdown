---
layout: post
title: "I Like My Coffee Strong and My Params Stronger"
date: 2013-11-06 21:16
comments: true
categories: rails, ruby
published: false
---

mass assignment in ruby and strong parameters
why the strong parameters part exists in scaffolding

I like my coffee strong ... and my params stronger.
I like my coffee the same way I like my params: strong.
I like my coffee and params strong
strong: coffee && params

mass assignment
dynamic dispatch - link to emilys post here
using the send method since it doesn't know which param it will get it ... and it ultimately uses the method_name= method
creates a white list of acceptable params
avi comes up with an interesting point about modified html headers ... if its modified then don't accept it

django does this already can we just adopt it in rails.

what are you thoughts? sound off in the comments

https://github.com/rails/strong_parameters
http://pivotallabs.com/rails-4-testing-strong-parameters/
http://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html
http://weblog.rubyonrails.org/2012/3/21/strong-parameters/
http://emilyxxie.github.io/blog/2013/11/02/defining-the-undefined-dynamic-definition-for-ruby-newcomers/
http://emilyxxie.github.io/blog/2013/10/14/dynamc-dispatch-101/
https://docs.djangoproject.com/en/dev/topics/signing/

strong parameters
duh had to fight a lot of people to force them to use this
new in rails 4

mass assignment vulnerability
around a year and a had ago github got hacked
lets pretend I'm on a registration form
they are a rails app air bob

lets try to imagine their users table
it looks like it has a first name column, last name column, ... based on the form
what if i guessed that it had a column named role ...
what if i further guessed that 1 would mean admin role in the role colum
the way they're probably processing this sign up form
then i could just change the form in the nested key

@user =  User.new(params[:user])
then later on in the controller they're probably doing something like this:

@user.save

I know the reason that they are prefixing all the form fields with user ... is so that they can glob params and just say params[:user]
here's the problem with html.
the client has access to the form field .. he/she can change it ... i can make the email address column submit to user[role] instead of user[email.]

Now when I submit a user, even though they didn't intend or present me with an ability to set my role ... when I submit this ... I will be an admin.  That's a big problem no?

This is the vulnerability of mass assignment

with great convenience comes great insecurity
never trust parameters from the scary internet. only allow the whitelist

we build a method called resource_params
it becomes a private method
w'ere gonna clean this hash of anything we might not want to pass along to the objects mass assignment

params should require a sub key of resource ... that permits only the following keys to pass through ...

so if i hack my form and use a different key .. it will sanitize the data ...

if someone manipulated my form i want to reject the whole form. if someone changed the html i sent them ... i sent your client html .. you manipulated it... i want to reject it

when we gen a form for rails we should be keeping on the server a SHA of the form. take html and encrypt it into a long string ... when you send me back the form i recncypt and make sure i mass the string i sent to begin with. its a signed form.  if you have signed forms you never have to worry about mass assignment vulnerableilities

django supports signed forms or gems for it

Let's take a look at a sign up form example:

<div align="center">
  <h2>Sign Up</h2>
  <form accept-charset="UTF-8" action="/create" class="signup-form" id="user_new" method="post">
    <input type="text" name="user[first_name]" placeholder="First name" value=""><br>
    <input type="text" name="user[last_name]" placeholder="Last name" value=""><br>
    <input type="email" name="user[email]" placeholder="Email Address" value=""><br>
    <input type="password" id="user_password" name="user[password]" placeholder="Password"><br>
    <input type="password" name="user[password_confirmation]" placeholder="Confirm Password"><br>
  </form>
</div>

Here is the markup used to create that form:

```html
<form action="/create" class="signup-form" id="user_new" method="post">
  <input type="text" name="user[first_name]" placeholder="First name" value="">
  <input type="text" name="user[last_name]" placeholder="Last name" value="">
  <input type="email" name="user[email]" placeholder="Email Address" value="">
  <input type="password" id="user_password" name="user[password]" placeholder="Password">
  <input type="password" name="user[password_confirmation]" placeholder="Confirm Password">
</form>
```