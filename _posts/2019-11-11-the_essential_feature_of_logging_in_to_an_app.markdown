---
layout: post
title:      "The Essential Feature of Logging In to an App"
date:       2019-11-11 17:45:21 -0500
permalink:  the_essential_feature_of_logging_in_to_an_app
---


Regardless of the capability of an app, it becomes extremely *incapable* if a  created user is unable to log in to the application. While the process of developing the login in feature may seem simple simple, a solid understanding of its functionality will not only give you a better picture of the app that you have developed, but may also determine whether you provide a smooth experience for your users or turn users away due to lack of access or security. Through the lens of a Sinatra application, I will walk you through the process and reasoning of developing a login feature for an app.

Developing a login feature will specifically require four files:
1. sessions_controller.rb
2. sessions/login.erb
3. users_controller.rb
4. user.rb
4. users/show.erb

Beginning in the SessionsController, a "GET" method is required with a '/login' route. This method should simply render the sessions/login.erb file, where the user will fill out the login form.

The login form will use a "POST" request within its method to send the user's input to the "POST" method in the SessionsController via the "/login" action once the form has been submitted. Here, the User model will utilize its ActiveRecord method "find_by(name: params[:name])" to capture the "name" that was input in the login form, setting it equal to a local variable, such as "user". Immediately following, the user is validated by its password to ensure that the password that the "user" submitted matches that which is stored within the application via the line "if user && user.authenticate(params[:password])". This capability is derived from the 'bcrypt' gem, which should be included in the app's Gemfile. This gem's ability to authenticate a user's submitted password through the login form to the "bcrypted" password already stored in the application through the "has_secure_password" mechanism within the User model.

Note: Some may assume that "user" should be made into an instance variable; however, this is unnecessary because the "user" will be redirected to "/users/#{user.id}" upon the successful act of logging in to the application and thus does not require the capabilities of an instance variable.

If the user has been successfully authenticated by 'bcrypt', the session[:user_id] is set equal to the user.id and the user is redirected to the "GET" method for its specific show page inside of the UsersController, via 'redirect "/users/#{user.id}"'. On the other hand, if the conditions have not been met, the "sessions/login" view will be rendered once again.

Once the user has been directed to the "GET" method for its respective show page in the UsersController, an instance variable of @user is set equal to the specific user that is found by id through "User.find_by_id(params[:id])". Upon successfully setting @user equal to the specific user that was matched through id, the "users/show" page is rendered for that user, displaying the provided information within the view file.

Note: In contrast to "user" in the post '/login' method within SessionsController, it is appropriate to utilize an instance variable with "@user" in the get '/users/:id' inside of the UsersController because the "users/show" page is simply rendered rather than being redirected as is the case within the post '/login' method.

Though developing a login feature may seem easy, I strongly recommend thoroughly examining the pathway that is developed in order to deliver the desired user experience.




