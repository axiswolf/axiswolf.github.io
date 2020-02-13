---
layout: post
title:      "Sinatra Project- The Hidden Cafe"
date:       2020-01-28 03:03:34 -0500
permalink:  sinatra_project_the_hidden_cafe
---


A little bit of background: I started 3 Sinatra projects trying to find one that is realistic to finish, but also challenges me.
My initial project was one for my mother- an appointment organizer so she would have a better time communicating with her customers. However, I find that there are some security flaws in sinatra and didn't want to take the risk. After a few tries and fails, I decided to make a sinatra app dedicated to a profession that funded my coding journey.

The Hidden Cafe. 

Project Requirements: 
1. Build an MVC Sinatra application.
2. Use ActiveRecord with Sinatra.
3. Use multiple models.
4. Use at least one has_many relationship on a User model and one belongs_to relationship on another model.
5. Must have user accounts - users must be able to sign up, sign in, and sign out.
6. Validate uniqueness of user login attribute (username or email).
7. Once logged in, a user must have the ability to create, read, update and destroy the resource that belongs_to user.
8. Ensure that users can edit and delete only their own resources - not resources created by other users.
9. Validate user input so bad data cannot be persisted to the database.

I started this project by making my MVC directories in the "app" folder.
Models --> Products, Users
Views --> Users (folder), Products (folder)
Controllers --> Application Controller, User Controller, Product Controller
Helpers --> Helper classes to make my life easier and keep track of users 

+ Gemfile: for all required gems
+ public (folder): just in case I wanted to make my project look nice
+ db (folder): for my ActiveRecord migrations
+ config (folder)
+ Rakefile

**Functions**
- My first task is to make sure my models and databases were talking to one another.
- So I purposed that users had many products and products belonged to user.

```
# app/models/users.rb
class Users < ActiveRecord::Base
has_many :products

end

# db/migrate/create_users.rb
class Users < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
    end
  end
end

```

```
# app/models/products.rb
class Products < ActiveRecord::Base
belongs_to :users

end

# db/migrate/create_products.rb
class Products < ActiveRecord::Migration[5.1]
  def change
    create_table :products do |t|
      t.string :name
      t.string :category
			t.string :brand
			t.decimal :price, :precision => 3, :scale => 2
    end
  end
end
```

Then, I started with my ApplicationsController by enabling sessions.
I create 'get' methods for my /, /signup, /login, and /logouts in addition to 'post' methods for my /signup, and /login pages.


```
class ApplicationController < Sinatra::Base 
enable :sessions
 
get '/' do

end

get '/signup'  do 

end

get '/login' do

end

get '/logout' do

end

post '/signup' do

end

post '/logout' do

end

end

```

We deal with these one at a time. For example:

When we get '/' page, who is viewing the home page?
What are they suppose to see?

- a user that is logged in
- a user that is not logged in

To address this, we need to build two Helper methods called 'current_user' and 'logged_in?'
```
class Helpers

    def self.current_user(session)
      if session[:user_id] != nil
        User.find(session[:user_id])
      end
    end
  
    def self.logged_in?(session)
      if session[:user_id] != nil
        user_id = current_user(session).id
        session[:user_id] == user_id ? true : false
      end
    end
  
    def self.current_product(id)
      Product.find(id)
    end
  
  end
```

Therefore, my end result will be:


```

    get '/' do
      if Helpers.logged_in?(session)
        @user = Helpers.current_user(session)
        @products = Product.all
        erb :'/products/index'
      else 
        erb :'/users/login'
      end
    end
```



- If the user is logged in, set the current user and redirect to the product page
- If the user is not logged in, redirect to the user login page






