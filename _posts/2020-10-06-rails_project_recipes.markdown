---
layout: post
title:      "Rails Project: Recipes"
date:       2020-10-06 22:49:07 -0400
permalink:  rails_project_recipes
---


> **Requirements**
> Use the Ruby on Rails framework.
> 
> Your models must:
> 
> • Include at least one has_many, at least one belongs_to, and at least two has_many :through relationships
> 
> • Include a many-to-many relationship implemented with has_many :through associations. The join table must include a user-submittable attribute — that is to say, some attribute other than its foreign keys that can be submitted by the app's user
> 
> Your models must include reasonable validations for the simple attributes. You don't need to add every possible validation or duplicates, such as presence and a minimum length, but the models should defend against invalid data.
> 
> You must include at least one class level ActiveRecord scope method. a. Your scope method must be chainable, meaning that you must use ActiveRecord Query methods within it (such as .where and .order) rather than native ruby methods (such as #find_all or #sort).
> 
> Your application must provide standard user authentication, including signup, login, logout, and passwords.
> 
> Your authentication system must also allow login from some other service. Facebook, Twitter, Foursquare, Github, etc...
> 
> You must include and make use of a nested resource with the appropriate RESTful URLs.
> 
> • You must include a nested new route with form that relates to the parent resource
> 
> • You must include a nested index or show route
> 
> Your forms should correctly display validation errors.
> 
> a. Your fields should be enclosed within a fields_with_errors class
> 
> b. Error messages describing the validation failures must be present within the view.
> 
> Your application must be, within reason, a DRY (Do-Not-Repeat-Yourself) rails app.
> 
> • Logic present in your controllers should be encapsulated as methods in your models.
> 
> • Your views should use helper methods and partials when appropriate.
> 
> • Follow patterns in the Rails Style Guide and the Ruby Style Guide.

I'm creating a simple web application using rails to keep track of user recipes and their favorite recipes
- Only users who wrote the recipe will be allowed to edit or delete
- You must have an account to create a recipe
- Any user can favorite a recipe
- Any user can create a category and only the creator gets to delete the category

The models for this assignment are:
- Recipes
- Users
- User_Favorites
- Ingredients
- Categories

Model relationships:
- Recipes have many ingredients and belongs to a category
- Categories have many recipes
- User favorites belongs to user and recipes (will have recipe_id and user_id)
- User has many recipes, and favorite recipes through user favorites

Controllers:
-User
-Categories
-Sessions
-Recipies
-UserFavorites

Database:
User
- username, password, first_name, last_name
User Favorite
- recipe_id and user_id
Recipe
-name, ingredients
Ingredients
-name, recipe_id
Category
-name, recipe_id

Nesting for user/recipe
**config/routes.rb**

```

  resources :users, only: [:show] do 
    resources :recipes, only: [:show, :index]
  end


```

