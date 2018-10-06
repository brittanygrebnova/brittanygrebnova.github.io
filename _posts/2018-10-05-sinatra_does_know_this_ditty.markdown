---
layout: post
title:      "Sinatra DOES know this ditty."
date:       2018-10-06 02:41:16 +0000
permalink:  sinatra_does_know_this_ditty
---


Finally!

My Sinatra portfolio project is nearing completion. My app is called B-Day Book, and it allows a user to log in and compile a collection of their birthday reminders by person and date. I hate missing someone's birthday and it happens quite often, so I thought this would be useful for other moms like me and anyone else trying to juggle 187 daily tasks. After a good bit of tinkering, all models, routes, and views behave as expected and what a relief. Writing the code for this project was not as challenging as finding the time to sit and do it, but that's a discussion for another day.

I'd like to use this opportunity to talk about some of the challenges that I did face and the concepts that I learned in debugging the errors that came up.

First, the session_secret! I found out the hard way that it's not until you do this:

```
set :session_secret, "some_cool_phrase"
```

that the session_id is created and can be assigned to the current user's id for the duration of the time they are logged in. Until I realized that this line was missing, the session_id changed at each redirect and I was getting a NoMethodError in the server. I didn't realize how important that step was until adding it made my whole program run correctly.

Next challenge was figuring out how to express the date attribute of the birthday. I decided to use SQL's date format over a string so that eventually I can add the functionality of displaying the upcoming week's birthday reminders. I haven't been able to figure that out yet but I plan to tackle it during the refactoring process. Something along the lines of, 'if @birthday.date is within Date.today + 7, display in a section called 'this week's birthday reminders'.  

I originally ran into trouble when I was using "text" as the input type in the form for adding a new birthday. When I found out I could do this:

```
<input type='date' name='date' placeholder='Date'></input>
```

I was finally able to get each of my birthday.date's to display properly. It also added a cute little calendar drop down to the form which looks really nice. I also utilized the type='password' for the login and signup forms. I like how these different input types utilize different formats and make the views look better.

The current challenge that I'm facing, and I don't know if I'll figure it out before my assessment or not, is getting the birthdays to display in chronological order. This sounds like a simple goal and it probably would be for another programmer, but I haven't figured it out yet. The reason is that the birthday.date's are converted from this format that comes in from the form '2013-09-25' to this format when displayed in a list 'September 25'. In order to list chronologically, I have figured that using Ruby's yday method would be a great chronological comparison, meaning the day of the year, but then I would need to create a method that compares the dates by day of the year but displays them in the month name and day format, 'September 25' for example. I need to build a method that compares the dates in one format and displays them in another.

I anticipate making a number of improvements to my portfolio project and can't wait to meet with an instructor who can tell me what needs to be added and improved. I believe that this is the most valuable part of these projects and I sincerely welcome the feedback. 

