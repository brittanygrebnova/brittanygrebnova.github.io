---
layout: post
title:      "DeliverMe: My Rails Portfolio Project"
date:       2019-01-28 11:39:33 -0500
permalink:  deliverme_my_rails_portfolio_project
---


And a testimony of the joy and sorrow of using Devise for user authentication.

I set out to recreate my very own (simplified) version of Postmates for the Rails Portfolio Project. The app is called DeliverMe and it 'delivers' the user an experience of choosing a vendor, placing an order, and having that order delivered. 

I set up my models like this:

```
class User < ApplicationRecord
  has_many :orders
  has_many :item_orders
  has_many :items, :through => :orders
end
```

```
class Vendor < ApplicationRecord
  has_many :items
  has_many :orders
end
```

```
class Order < ApplicationRecord
  belongs_to :vendor
  belongs_to :user
  has_many :item_orders
  has_many :items, :through => :item_orders
end
```

```
class Item < ApplicationRecord
  belongs_to :vendor
  has_many :item_orders
  has_many :orders, :through => :item_orders
end
```

One of the lessons I learned about ActiveRecord relationships during this project was the importance of correctly naming your join table, and also making sure to state the has_many relationship before has_many_through. In my case, I had originally titled my join table 'items_orders', had trouble getting the relationship working, then found a lesson that explained the naming convention of :singlemodelname_pluralmodelname. Combined with changing my relationships to read has_many :item_orders before calling has_many :orders/:items, :through => :item_orders, I was able to finally use methods like @item.orders and @order.items.

After I had my models and relationships set up on a basic level, it was time to figure out user authentication. Up until that point, I had only used bcrypt for managing users' passwords. After hearing and reading about the popularity of Devise I decided to figure it out and incorporate it into my app. Following the setup instructions was simple; include **gem 'devise'** in my Gemfile, run **bundle install** and then **rails generate devise:install**. Once the initializer was installed and I configured all the Devise options, it was time to generate my Devise models. I would need authentication for both Users and Vendors. I added the following to my User and Vendor models:

```
class User < ApplicationRecord
  devise :database_authenticatable, :registerable, :recoverable, :rememberable, :validatable, :omniauthable, :omniauth_providers => [:facebook]
end
```

```
class Vendor < ApplicationRecord
  devise :database_authenticatable, :registerable, :recoverable, :rememberable, :validatable
end
```

For the sake of simplicity, I chose not to allow a Vendor to sign in through Facebook. I may add that option during the refactoring process. 

The thing I had the most trouble with was following the redirects after each step that was provided by Devise. A lot of the magic provided by Devise is hidden inside files that need to be generated in order to modify them. For instance, in order to modify the sign in page, I needed to run the command **rails generate devise:views**. This gave me access to the views/devise directory within which are folders titled sessions and registrations that contain the view code for the sign in and sign up pages. It was here that I could add options to sign in/up as a vendor from the users sign in/up page and vice versa, a detail that I believe adds user-friendliness. 

Another Devise learning opportunity was provided when trying to modify the after_sign_in, after_sign_out, and after_sign_up paths for both Users and Vendors. After trying out a few different tips, I found that this logic could be encapsulated within methods defined inside the ApplicationController. I got these two methods to work:

```class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def after_sign_out_path_for(resource_or_scope)
    if resource_or_scope.is_a?(User)
      new_user_session_path
    elsif resource_or_scope.is_a?(Vendor)
      new_vendor_session_path
    else
      root_path
    end
  end

  def after_sign_in_path_for(resource_or_scope)
    if resource_or_scope.is_a?(User)
      user_path(current_user)
    elsif resource_or_scope.is_a?(Vendor)
      vendor_path(current_vendor)
    end
  end
end
```

This allowed me to redirect to a page other than the root_path after sign_in and sign_out. The kicker, however, is that I still haven't figured out how to redirect to my desired path after sign_up. I assumed after reading around that this code would work:

```
  def after_sign_up_path_for(resource_or_scope)
    binding.pry
    if resource_or_scope.is_a?(User)
      add_user_profile_path(current_user)
    elsif resource_or_scope.is_a?(Vendor)
      add_vendor_profile_path(current_vendor)
    end
  end
	```
	
But it doesn't. Not yet, anyway. I compromised and changed my users/show and vendors/show views to include the link to 'add info to profile' instead of redirecting straight to this view, but I would still really like to make this work the way I originally intended. Another task for the refactoring process. 

Though it was slightly painful and more time-consuming than I had anticipated, I enjoyed the process of building my first Rails app and learning how to use Devise for user authentication.

Thanks for reading!

