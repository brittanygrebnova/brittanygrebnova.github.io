---
layout: post
title:      "Class vs. Instance"
date:       2018-06-15 17:51:40 -0400
permalink:  class_vs_instance
---

In order to strengthen my understanding of this topic, I have read quite a few blog posts and watched many a tutorial (thank you, internet!). I've also written some code to showcase my knowledge of the concept of class vs. instance methods and variables which I will share here. Let's go!

When trying to come up with a coding concept, I decided to create a Family class that would allow me to create new families, give them family names and members, and add them to a community of Families. My code starts out by defining the Family class this way:

```
class Family

attr_accessor :name, :location, :mama, :papa, :kids
```
For all intents and purposes, these are the attributes of an instance of a Family: name, location, mama, papa, and kids. By the way, that sentence opens up the first concept to understand regarding classes and instances. Each family that gets created is an instance of the Family class. Each time a new family is created, this code is executed: 

```
@@community = []
@@family_count = 0

def initialize(name)
  @name = name
	@members = []
  @@family_count += 1
  @@community << name
end
```

So when I enter,

```
grebnovs = Family.new("Grebnov")
```

a new instance of the Family class is returned, which looks like this:

```
#<Family:0x000055b3c0b52df8 @name="Grebnov", @members=[]>
```


This is an instance of the Family class, complete with the name and empty members array that it was initialized with. By initializing this instance of the Family class, I've also increased the family_count by 1 and added the family name to my community of Families.

Now that we have one instance of our Family class, we can add members. This is done by entering these lines:

```
grebnovs.add_member("Arseniy")
grebnovs.add_member("Brittany")
grebnovs.add_member("Mila")
grebnovs.add_member("Damir")
```

which enacts the following code:

```
def add_member(member_name)
  @members << member_name
end
```

The method above is an instance method because we are adding a member to an instance of the Family class (this one: #<Family:0x000055b3c0b52df8 @name="Grebnov", @members=[]>). Each time a member_name is added, that string is shoveled into the @members instance variable which is set equal to an empty array upon initialization. Each instance of Family should keep track of its own members, which is why the @members variable is an instance variable and not a class variable. Through trial and error I came to understand that if it were a class variable defined as an empty array before initialization, i.e. @@members = [], it would hold the members of all the instances of the Family class instead of those from a specific family.

Now that we've added members to our family, my code has the functionality to find a member of the family in case, let's say, one of the kids were to get lost or one of the parents were needed. This capability is encapsulated within this instance method:

```
def find(member_name)
  if @@members.include?(member_name)
    "Here's #{member_name}!"
  else 
    "Keep looking!"
  end
end
```

So, when we enter

```
grebnovs.find("Mila")
```

we are returned

```
"Here's Mila!"
```

#Find is an instance method because, again, we are looking through an instance of the Family class for one of its members.

Now we can also create additional instances of the family class. Each time a new family is created, it gets a name and an empty members array. By creating additional instances of Family, our @@family_count variable increases by 1 and the family name is added to our @@community array. If we want to see how many families have been created, we enlist the following class method:

```
def self.count
  @@family_count
end
```

and if we want to search our community for a specific family name, we can type into the terminal:

```
Family.find("Smith")
```

and get the message:

```
"Pleased to meet you! We're the Smith's"
```

A simple bit of code but a really valuable exercise for me to understand this concept better. Use an instance variable and method when operating on an instance of the class you've created, and use a class variable and method when you need to operate on the entire class itself, as in the case of a find or count method.

I'm so relieved to have gone through this additional research in order to better grasp the concept of class vs. instance variables and methods. You can see my code in its entirety below. Thank you for reading!

```
class Family

attr_accessor :name, :location, :mama, :papa, :kids

@@community = []
@@family_count = 0

def initialize(name)
  @name = name
  @members = []
  @@family_count += 1
  @@community << name
end

def add_member(member_name)
  @members << member_name
end

def members
  @members
end

def find(member_name)
  if @members.include?(member_name)
    "Here's #{member_name}!"
  else 
    "Keep looking!"
  end
end

def self.count
  @@family_count
end

def self.find(family_name)
  if @@community.include?(family_name)
    "Pleased to meet you! We're the #{family_name}'s"
  else
    "Looks like the #{family_name}'s haven't joined the community yet."
  end
end

end
```









