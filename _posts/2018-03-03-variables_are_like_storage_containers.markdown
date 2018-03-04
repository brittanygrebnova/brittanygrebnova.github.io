---
layout: post
title:      "* "# Variables are like storage containers!"``"
date:       2018-03-04 03:41:02 +0000
permalink:  variables_are_like_storage_containers
---


Now, my understanding of variables (and most other programming concepts so far) is basic and just developing, but I have come to appreciate the variable for the handy little storage container that it is. 

When used for string interpolation, the variable can be conveniently injected into a string such as:

name = mommy

"#{name}!! Can I have some more chocolate? Pleeeease #{name}!!"

And not only that. When you need to store multiple strings or other pieces of data, the variable becomes the title of your array. For example:

days_of_the_week = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]

And referencing the variable with a chosen index allows you to access any of the elements inside your array easily, like this:

days_of_the_week[0] == "Monday"

The importance of the variable's function to store information, specifically in the case of iteration, became quite apparent to me in recent labs. For instance, in the badges and schedules lab, I used the variable badge_messages within my #printer method to store the return value of my #batch_badge_creator method. From there, I could iterate over each of the elements and print them out as required. Take a look:

def printer(attendees)
  badge_messages = batch_badge_creator(attendees)
  badge_messages.each do |x|
    puts x
  end
  room_assignments = assign_rooms(attendees)
  room_assignments.each do |x|
    puts x
  end
end

Couldn't have done it without variables. I've used them many times in labs so far and will use them many, many more times in the future. They're convenient, powerful, and fun to name!

Thanks for reading!





