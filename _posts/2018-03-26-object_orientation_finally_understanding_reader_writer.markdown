---
layout: post
title:      "Object Orientation: Finally Understanding Reader/Writer"
date:       2018-03-26 18:27:53 +0000
permalink:  object_orientation_finally_understanding_reader_writer
---

The introduction of object orientation in Ruby seems to be well-timed and intuitive within this course. The ways that we learned to build methods before are now making sense in the context of a class. Before learning about object orientation, I kept wondering where all these methods were going to be used, how they tied together, and what was their ultimate purpose. Now that object orientation has become the name of the game, it's clear that we use these methods to bring objects to life. 

One of the most recent 'Aha!" moments that I've had was related to the reader and writer methods that are involved in building a class. When OO was first introduced, and the process of defining the #name= and #name methods was being taught, I felt like I was mechanically typing the same thing over and over without comprehending what was happening. Why did we need two different methods to represent the same variable? And what does that = sign do? I didn't get it. 

Then, somewhere along the way, I realized that when we call name= (or any variable=) in our program, we are setting that variable equal to a value for a particular instance of a class. We are *writing* that variable's value. We also need an additional method to recognize that value and allow it to be referenced and operated on with all the capabilities that the class has to offer, hence the necessity for the *reader* method that includes the @variable, the instance variable. The instance variable is recognized throughout the scope of the class.

I finally wrapped my mind around this concept when along came attr_accessor to replace these two methods! Well, not always, but in many cases. Writing out the reader and writer methods is the foundation, and using attr_accessor is the next level of abstraction that streamlines our code and makes our lives easier. I get it!

Thank you for reading : )
