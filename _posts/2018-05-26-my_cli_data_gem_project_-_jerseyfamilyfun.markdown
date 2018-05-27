---
layout: post
title:      "My CLI Data Gem Project - JerseyFamilyFun"
date:       2018-05-27 03:01:35 +0000
permalink:  my_cli_data_gem_project_-_jerseyfamilyfun
---


The last of the final projects in our Ruby section, the CLI data gem project required us to create our own command line application that scrapes a site of our choice and returns information to the user from that site. For my project, I chose the website www.njkidsonline.com which offers a daily list of family-oriented events throughout the state of New Jersey. Here's a little walkthrough of my program.

The user runs ./bin/jersey_family_fun which triggers this line:

JerseyFamilyFun::CLI.new.call

The call method and associated methods within look like this:

```
def call
    list_events
    menu
    goodbye
end
```
	
```
def list_events
    puts "Welcome to Jersey Family Fun! Here is a list of today's events:"
    @events = JerseyFamilyFun::Event.today
    @events.each.with_index(1) do |event, index|
      puts "#{index}. #{event.title_and_location}"
    end
end
```

The list_events method creates an @events instance variable using the event class method self.today, and iterates over each of those instances to output the number of the event and its title and location.

```
def self.today
    self.scrape_events
end
```

And self.scrape_events is as follows:

```
def self.scrape_events
    doc = Nokogiri::HTML(open("https://www.njkidsonline.com/events"))
    events = []
    doc.css("div.ListingItem").each do |listed_event|
      event = self.new
      event.title_and_location = listed_event.css("h3").text.strip
      event.date_and_time = listed_event.css(".FieldRow").text.gsub("Date", "").strip
      event.url = listed_event.css("a").attribute('href').value
      #binding.pry
      events << event
    end
    events
end
```

This is where the information about the daily events is scraped from the site and given the attributes needed for the program. The return value is an array of events: the array that is iterated over in the list_events method. The next part of the program is the menu which accepts the user's input and returns the title and location as well as date and time and url of the chosen event. The user can choose any number from the ordered list, can enter 'list' to see the list again, or 'exit' to exit the program, at which the program outputs "Come back tomorrow for more family fun events!". 

```
def menu
    input = nil
    while input != 'exit'
      puts "Enter the number of the event you'd like more information about, or 'list' to see today's list of events, or 'exit':"
      input = gets.strip.downcase
      
      if input.to_i > 0
        event_chosen = @events[input.to_i - 1]
        puts "#{event_chosen.title_and_location} - #{event_chosen.date_and_time} - #{event_chosen.url}"
      elsif input == "list"
        list_events
      else
        puts "Please enter the number of the event you want to learn more about, 'list', or 'exit'"
      end
    end
end
```
  
```
def goodbye
    puts "Come back tomorrow for more family fun events!"
end 
```

While the program is incredibly simple, I'm proud to have written it and to have accomplished something that seemed impossible at first.

Thanks for reading!
	
	
