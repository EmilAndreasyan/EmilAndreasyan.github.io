---
layout: post
title:      "Power of Logic"
date:       2019-10-01 00:20:51 +0000
permalink:  power_of_logic
---


In my homeland, I graduated from Medical University, but in the USA you should study over again to become a doctor. So I had to decide to which occupation I am to dedicate my time and energy and for what kind of job I can be proud of.  From different ramifications, I decided to select programming courses for several reasons. Putting it programmatically, you can write a method called "career_builder", which gives you different pieces of advice based on data you insert, such as:  name, age, and occupation. Let's put it in Ruby way:

```
def career_builder(name, age, occupation)
if age <= 18 && occupation == ""
puts "Go to college, #{name}, to get education"
elsif age >= 18 && age < 30 && occupation == ""
puts "You are old enough, it's time to learn some stuff!"
elsif age >= 18 && age < 30 && occupation != ""
puts "Congratulations, #{name}! You are now #{age} years old and you have become #{occupation} already!"
elsif age >= 30 && occupation == ""
puts "Oh, #{name}... It's never too late to study though..."
elsif age >= 30 && occupation != ""
puts "Well done, #{name}! You're #{occupation} now, work hard!"
end
end
career_builder("Amanda", 30, "web developer")
```

What is great about programming is that you can perform some magic by changing data dynamically and extrapolating the results' output.
As a lifetime profession, it requires intelligence, precision, constant learning, creativity and a big deal of logic. It is difficult to find another profession which offers such versatile activities and the graduates of which not only engineer software, but also their own personality. At a first glance, I thought that programming is boring and fits to introverts. For me it was like learning Chinese or any other language I am totally unfamiliar with. But as any other language people speak around the world, it requires energy to gradually learn letters, words, try to compose first sentences. But after that, you start to enhance your sentences and ultimately write sophisticated novels! As a true writer, you elaborate every detail, you plan, write, change, erase until your thoughts are materialized and your code runs seamlessly.  As immersion may reveal hidden treasures, one should dive really deep to grasp them.
Another metaphor for programming is like building a house. You should elaborate building plan with colleagues and make sure that all steps occur in the right order! First, you should make a foundation so that your home has something to lean upon. Then, you are to build the walls, windows, rooms. In the end, you should have a roof. Seems easy? It is easy only when every step is meticulously planned and it's is often impossible if you mix stages even doing all the separate steps correctly! For instance, in real life, if you put the foundation upon the roof, it will not work out, the house will probably collapse, but in programming, it will imminently throw an error.
Unlike majority of professions, programming is not mainly based on experience, but on constant learning. You can learn all the methods within a month or two, but it requires years to master them and put into action interchangeably. This is why you'll never be bored and feel yourself stagnating. Initially, you starts with simple markup language and end up becoming full-fledged Software Engineer. It can take you to an unforgettable journey from being know-nothing to becoming a rock star, from dead end to the breakthrough when you finally understand what you write and how to improve your code.
Although I have only touched the tip of the iceberg, I can assume, that programming consists of 4 major parts, listed down from most to least difficult:

1. Understand the concept (most difficult)
2. Understand the logic
3. Elaborate steps
4. Write the syntax (free of errors and most abstract)

Different people have different skills, I believe that every person has a talent yet to be discovered, but, let’s be honest, not everyone can or should code… And those who indeed can, have an unspoken obligation to make a contribution in the development of Software Engineering. For me personally, it's the power to **create, manipulate and enhance.**
If one decides to learn a new language and doesn't know which one to pick, I would firmly recommend to learn that of programming one!

