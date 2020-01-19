---
layout: post
title:      "Pieces of the puzzle"
date:       2020-01-19 04:50:57 +0000
permalink:  pieces_of_the_puzzle
---


Learning can be both hard and interesting, but implementing knowledge and having a working application might reward with exciting results.
As every hard thing consists of many easy pieces, I will try to break down the project and scrutinize it step by step, following a logical path from having an idea to completing a functioning web application.
Step 1. Coming up with an idea.
A captivating idea is the first and often, the most difficult part of the project. We know vivid examples of people having great ideas and starting to earn fortunes. When one tries to generate a distinguishing idea, one should answer some basic questions: why is it important? How this idea will facilitate the lives of others? How my idea is different from those of others? Is that difference beneficial? So it's all about altruism, about helping other people. My idea was to create a helping tool for a specific group of people, for physicians, while their work is both important and difficult. As long as they might have trouble tracking all the information about their work, I wanted to help them find the necessary information about their patients, diseases and prescription drugs to optimize the treatment process.
Step 2. Databases
So I decided to make four databases and connect them all so they can be used interchangeably. As a helping tool for drawing databases is draw.io site, and I was able to connect the `has_many` and `belongs_to` dots visually (https://www.draw.io/#G1SmainEGVimzxUQd-2mm1vwSwlVPlIYWr). Doctors (Users in our case) are the parent element, who has many patients, many diseases and many drugs, through patients. This step is important for having a clear vision and not mixing everything up later on. Using ActiveRecord, four appropriate tables were created. A few important things are to be highlighted here. First of all, User should have `password_digest` attribute in its table (along with pre-installed gem 'bcrypt') instead of mere `password`, for security reasons, so the account is less hackable. Secondly, every table that `belongs_to` other table must have `t.integer :<other_table's_name>_id`. For instance, Patients' table should have `t.integer :user_id` and `t.integer :disease_id`, and Drugs table has `t.integer :patient_id`, in its turn. After creating tables, they must be migrated.
Step 3. Models
Models are the classes that we learned thoroughly throughout the curriculum and, due to inheritance from `ActiveRecord::Base`, we can avoid writing `def initialize`, `all = []`, `def save`, etc. They are all (and many other things) come for free with ActiveRecord! So, our 4 classes (which have 4 appropriate tables) have few important things that must be indicated. `Has_many`, `has_many, through:`, `belongs_to ` macros (if you forget this connection, you can always look through the tables), `has_secure_password`, `validates_presence_of`.
Step 4. Controllers (the heart of MVC - models, views, controllers).
The controllers are the most important part of all Sinatra project, where most of the logic and all the navigation is performed. The heart of controllers is `ApplicationController`, which inherits from `Sinatra::Base`, and all other controllers, in their turn, should inherit from `ApplicationController` (the heart of the heart). This is where the sessions should be enabled, and Helpers methods can be elaborated. Not describing all other Controllers, at least two of them should be pointed out: `SessionsController` and `UsersController`. The first is responsible for logging in and out, and the second one is for signing up (we can write everything in one Controller, but this is how a logical convention suggests). In other controllers, the concept of CRUD (create, read, update, destroy) must be implemented. In our `get` requests, we must underline routes, connect to views, create instance variables that we will use in those views, and write most of the logic. For instance, in `PatientsController`, we can write: 

```
get '/patients' do
       require_login
       @patients = Patient.all
       erb :'patients/index'
end
```

Adding logic is the most challenging part of Controllers, but, at least, we have two helping tools: `params` hash and `binding.pry` for debugging. For instance, when failing to answer to the `post` request (create) error message should be raised and the page should be redirected to a new "create" event.
Step 5. Views
Views are `erb` files (embedded Ruby) where all our routes and logic is visible to the users. For example, in `new.erb` we can *C*reate, in `show.erb` we can *R*ead, in `edit.erb` we can *U*pdate, and in index.erb we can see all of them and also *D*estroy them (do you follow the CRUD pattern here?).
Step 6. CSS (Cascading Style Sheets). After having all the hard work done, we can "reward" ourselves with writing some more code and bringing a little beauty. Few users would want to use our application only for its functionality, but most of them would if we add some unique style, beauty, and enhanced visibility. This is where CSS comes in! Of course, everyone could have its perception of style, but most users would agree, that everything would be better than plain HTML's boring black and white texts and forms.
All steps. Throughout all the stages of building the applications that users can benefit from, troubleshooting the code is altogether important! Although it's annoying to encounter different errors all the time, get stuck most of the time and end up in frustration, but this is how one learns and makes progress. Remember to treat errors as teachers, which help you become a more intelligent and professional developer!
Though not everything went as smoothly and swiftly as I would want it, when all the pieces of the puzzle came up together, the results made me proud and rewarded with an application (), which can make some people's lives a bit easier!

https://github.com/EmilAndreasyan/HealthNet
