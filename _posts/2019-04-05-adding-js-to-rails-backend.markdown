---
layout: post
title:      "Rails with Javascript Portfolio Project"
date:       2019-03-25 16:20:05 +0000
permalink:  rails_with_javascript_portfolio_project
---

Rails with Javascript Portfolio Project

I didn’t think it was possible to make my Rails app, DeliverMe, any better than it already was…..

Just kidding, but I did add some Javascript features to it and it’s really cool. The object of this final project was to hijack a few of the different functionalities in my existing Rails application and use Javascript to render information on the page without a refresh.

There were a number of road blocks that required a significant amount of focus and research. Here I’ll detail a few of them and also link to my repository.

First off, where do I even start? This question stalemated the project’s execution until I broke the problem down to a series of small steps. I got a lot of guidance from the walkthrough videos made available from Flatiron alumni. I finally understood that I needed to start by adding an eventListener for one of my existing Rails buttons, and from there, prevent the default action and write a function to display the requested information on the same page. I started with the button that would display all the orders for a single user. I put the listener function inside $(document).ready like so:

```
$(document).ready(function() {
  $("button#user-orders").click(function(e) {
    e.preventDefault();
    console.log("default prevented!");
    getOrders();
  })
```

OK. Default (redirecting to the user’s orders index page) prevented, console.logged a message to confirm, and triggered my getOrders function:

```
function getOrders() {
  $.getJSON( 'https://localhost:3000/orders', function(data){
    console.log("the data is: ", data)
    data["orders"].forEach(function(order) {
      let userOrder = new Order(order);
      let userOrderHTML = userOrder.orderHTML();
      document.getElementById("user-orders-list").innerHTML += userOrderHTML
    })
  })
}
```

This function sends a getJSON request to the user’s orders index page, retrieves the JSON data and iterates over it. Using a class constructor, I create a new instance of Order and use my prototype method, orderHTML, to format the data and append it to the DOM. Yay!

The next tricky part was using Javascript to render form data. I ignorantly spent days writing a 20+ line function that would dynamically create a form using pure Javascript, until I met with one of the section leads who clarified that I should instead be hijacking my existing Rails form and using Javascript to display the data instead. Great! After understanding that requirement better, it was a much slimmer amount of code that got the job done. It starts with another eventListener for a click on the button to place a new order:

```
$("form.new_order").on("submit", function(e) {
    e.preventDefault();
    console.log("default prevented!");
    const values = $(this).serialize()
    placeNewOrder(values)
  })
```

And then this function is run:

```
function placeNewOrder(serializedValues) {
  $.ajax({
    url: 'https://localhost:3000/orders',
    type: 'post',
    dataType: 'json',
    data: serializedValues,
  })
  .done(function(data) {
    $('#results').html('')
    console.log(data)
    const newOrder = new Order(data["order"])
    const newOrderHTML = newOrder.orderHTML()
    $('#results').html(newOrderHTML)
  })
}
```

The order gets submitted and what’s rendered to the DOM is the order’s show page. Beautiful!

After getting over the idea that this would be the most difficult thing in the world, I had fun adding JS to my Rails application. It was interesting to see how they work together and definitely improved my understanding of both.

One cool thing to mention is that I was able to help a fellow schoolmate get one of her functions up and running when she ran into a wall. She was using fetch for her JSON requests, which I didn’t use and wasn’t as comfortable building. We worked through the code over a screenshare and by the end got her application to behave as she wanted. A first for me!

Thanks for reading : )

My project’s github repository link:
https://github.com/brittanygrebnova/deliverme
