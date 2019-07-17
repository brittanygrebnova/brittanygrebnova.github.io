---
layout: post
title: "5 Interesting Ruby Interview Questions and My Answers"
date: 2019-07-15 02:25:01 +0000
permalink: how-im-maintaining-positivity-while-job-searching
---

In preparation for software engineering interviews, I’m focusing on getting really good at answering questions about stuff that I already know. I have also been carving out time every day to learn new libraries/frameworks, but for the sake of feeling confident, I want to keep the languages I’ve already learned at the forefront.

I’ve taken 5 questions that I believe are the most interesting from this article: https://rubygarage.org/blog/how-to-interview-your-ruby-on-rails-developer and share my answers to them below.

<strong>1. What is the difference between a class and a module?</strong>

While both classes and modules can hold a collection of methods and constants, a class is capable of instantiating an object and a module is not.

<strong>2. Describe the difference between class and instance methods?</strong>

One way to describe the difference between class and instance methods is this: a class method is a method whose receiver is the class and an instance method is a method whose receiver is an instance. A class method will be performed on the entire class (all instantiated objects) while an instance method will act on a single instance of the class.

<strong>3. Provide an example of RESTful routing and controller.</strong>

RESTful routing is a system of mapping client requests to their corresponding actions. We utilize HTTP verbs and connect them to the controller which performs the requested action and points to the view that should be returned to the client. Here’s an example of RESTful routing for an application that allows the user to create and manage their photos:

<strong>GET ‘/photos’</strong>

<p style="margin-left: 60px">Points to the index action in the photos controller</p>
<p style="margin-left: 80px">Displays a list of all the user’s photos</p>
<strong>GET ‘photos/new’</strong>
<p style="margin-left: 60px">Points to the new action in the photos controller</p>
<p style="margin-left: 80px">Returns the form for the user to create a new photo</p>
<strong>POST ‘/photos’</strong>
<p style="margin-left: 60px">Points to the create action in the photos controller (after a user submits the ‘new’ form)</p>
<p style="margin-left: 80px">Creates a new photo
<strong>GET ‘/photos/:id’</strong>
<p style="margin-left: 60px">Points to the show action in the photos controller</p>
<p style="margin-left: 80px">Displays the photo with an id that matches that of the URL</p>
<strong>GET ‘/photos/:id/edit’</strong>
<p style="margin-left: 60px">Points to the edit action in the photos controller</p>
<p style="margin-left: 80px">Returns the form that allows the user to edit the photo with an id that matches that of the URL</p>
<strong>PATCH/PUT ‘/photos/:id’</strong>
<p style="margin-left: 60px">Points to the update action in the photos controller</p>
<p style="margin-left: 80px">Updates the requested photo after user submits the ‘edit’ form</p>
<strong>DELETE ‘/photos/:id’</strong>
<p style="margin-left: 60px">Points to the delete action in the photos controller</p>
<p style="margin-left: 80px">Deletes the photo with an id that matches that of the URL</p>

<strong>4. What is Object-Relational Mapping?</strong>

Object-relational mapping is a programming technique for accessing and modifying your database without writing direct queries like ‘SELECT’, for example. ActiveRecord is Ruby on Rails’ built-in ORM and allows us to make such calls as Model.create and Model.find.

<strong>5. How should you store secure data such as a password?</strong>

Encryption is key when storing securing data such as a password. The gem ‘bcrypt’ provides this security and can be implemented by requiring it in the gemfile and adding the line ‘has_secure_password’ inside your user model. Any instance of User will be generated with a password and password confirmation property and, as long as they match, the password is encrypted and stored as password_digest.
