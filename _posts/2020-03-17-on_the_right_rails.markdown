---
layout: post
title:      "On the right Rails"
date:       2020-03-17 04:55:20 +0000
permalink:  on_the_right_rails
---


As a further development and perfection of Sinatra comes Rails framework, which has various built-in functions that can make the life of a programmer a bit easier. It's all built on the concepts of convention and DRY (Do not Repeat Yourself). But if you think  that you'll write an application without making much effort, you should think twice.
As for my application workflow, I chose a mobile app for movies, where user can CRUD (Create, Read, Update, Destroy) movies, add artists and genres. In order not to mess things up, one should think of each steps that should be taken. First, the idea and mapping of has many, has many through and belongs to relationship. 
Let's put everything in a right order.
1. Relationship diagram with foreign keys, this is where you can graphically see how your tables should look like and what is the parent-child relationship (this comes very handy at later stages)
2. Having secure password. In order to protect user's sensitive data from unauthorized access, we need to change the simple password into solid one, with the macro of `has_secure_password`, bcrypt gem in our Gemfile, and `password_digest` inetger in User's table. This will convert and salt the password into unhackable hash, and even the developer will never know the exact password, but will always see its digested end product.
3. After migrating and seeding the database, next step that should be taken is creating models and adding all kinds of validations. Validations are the guards which either persist good, valid data into our database, or reject them if they have invalid attributes. Such validatations check presence of name, email, password, etc. Despite the fact, that some users may have same names and same passwords, they will never have same emails, that's why we should validate presence and uniqueness of email. also we can add some validations that check length, format, etc., which is really important to keep the database clean from invalid data.
4. After generating models, the next step is controller generations (or we can use `rails generate resource` instead, which will create tables, models, controllers, helpers, but not views). Controllers are responsibe for interaction between views and models, some sort of bridge, where the main logical part of iterations are written. There are 7 routes in Rails controller which make logical paths for us, such as `index`, `new`, `create`, `show`, `edit`, `update`, `destroy` . Rails is smart enough to know that we should use post http request for create action, get request for index, new, show, edit, destroy, put or patch reguest for update actions. To  make our life even easier, we can abstract our code away by adding `before_action` macro, which can write same code for every action we indicate in `only`: hash. For instance, we want to find an instance by its id, like @movie = Movie.find_by(id: params[:id]), which is common code for `show`, `edit`, `update` and `destroy` actions. At the top level of our controller, we can write
`before_action :set_movie, only: [:show, :edit, :update, :destroy`]
Also we should remember to add `set_movie` method after `private` keyword to keep them safe

```
private
def set_movie
    @movie = Movie.find_by(id: params[:id])
 end
```
In this case, we shouldn't manually write the same piece of code in each method, but we write this method only once and indicate where we want to use it with `only` keyword and array of actions
5. In routes folder, we should add all the routes we want user to be redirected to. As a convention, Rails uses `resource` for all the routes, `resource, only: []` for only those routes we want to display, custom routes such as `get '/login', to: 'sessions#new', as: 'login'`, and nested routes.
6. Views are the only parts in application, that users can  see, so be careful with what you display! As another convention, Rails provides us with two types of forms that we can benefit from in views, such as `form_for` and `form_tag`. Form tag is low level, which is common and can be used as a template everywhere (if we don't want to write these form fields each time, we can always render them from `_form`). In `form_tag`, we must explicitily designate the route and method (unless it's get request). `Form_for` is a top level form which privides with building block (f) and connected to the controller and every instance variable that the controller action might have. For instance, if in our new controller we write 
```
def new
@movie = Movie.new
end
```
```
we can write in our new.html.erb the follwing lines:
<%= form_for @movie do |f| %>
<%= f.label :title %>
<%= f.text_field :title %>
<%= f.submit %>
<% end %>
```
This is how we connect our view with controller via @movie instance varibale and, after hitting submit button, it will initiate post request to create action of the same controller, thus Rails predicts the workflow and abstracts thing away!
7. If you have some unique methods that you want to benefit from, Helpers can come very helpful as reusable pieces of code. Note, if you write your helper methods in ApplicationController, you should explicitly tell the browser about this methods so you could invoke them in the view (view doesn't know about controller method unless you tell them). As an example:

```
class ApplicationController < ActionController::Base
def current_user
     User.find_by(id: session[:user_id])
   end

   def logged_in?
    !!current_user
   end

 helper_method :current_user, :logged_in?
end
```
Before the closing tag, we have declared `helper_method`, so that we can have access of it from the views.
However, if you write your helper method as modules in helper folder, views with the same name already know about this method and you can always inject them in without the necessity of declaring them as helper methods (when generating in console a controller, it creates controllers, routes, views and helpers, which are associated with one another).
8. As another important part of the authorization comes OmniAuth, which ensures users registarion or logging in with the ability to do so via different third party sites  (strategies), as name suggests (Omni Authorization), such as Facebook, Twitter, GitHub, etc. In this case, our application sends the users to the mentioned sites, where they log in and are redirected back to our site, thus establishing connection (and trust) between user's data in third party sited and our application.
Building application with Rails might seem easy, but it appears to be challenging once you start working. But later, when you start grasp the concept and workflow, it rewards you greatly by making life easier, eliminating unnecessary code and landing state-of-the-art applications.

