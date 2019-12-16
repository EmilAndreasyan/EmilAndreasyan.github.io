---
layout: post
title:      "Common attributes of initialization"
date:       2019-12-13 16:15:48 -0500
permalink:  common_attributes_on_initialization
---


Object lifecycle is an essential building block of classes, triggered upon instance creation and inserting into memory some vital and repetitive data. It's one of the concepts of DRY (do not repeat yourself), which means, if something can be written with less code, without the necessity of repetition, it is usually done this way. Before jumping into examples of initialization (or object lifecycle, or hook), we should understand basic concepts of classes and instances. Classes are behavioral blueprints that all the instances have in common, like instance and class methods (the latter are written with `self.` unlike instance methods), and instances are individual class-based creations, which have some unique data. Let me illustrate my point on the following example:

```
class Person

 attr_accessor :name, :age, :gender

 @@all = []
 def initialize(name, age, gender)
 @name = name
 @age = age
 @gender = gender
 @@all << self
 end
 
 def self.speak
 puts "I am a general #{self} and I can speak"
 end
 
 def say_name
 puts "I am particular #{self.class} and my name is #{self.name}"
 end
end

derek = Person.new("Derek", 35, "male")
samantha = Person.new("Samantha", 30, "female")
```

This initialize method in our Person class is what called the object lifecycle. Unlike classes, which are common blueprints for every single instance created, object lifecycle has only common arguments, but different data. If I try to define this concept (or rather a functionality), I would say, that **Initialization requires all the instances to have common arguments with different values upon instantiation**. Based on real-life situation, we can make educated guess, that every instance of Person should have, at least, name, age, and gender on the moment of creation (common arguments), but it's filled with different types of data, like `"Derek"` vs `"Samantha"` or `35` vs `30`. Initialization is helpful, but also error-prone if we fail to pass arguments wrongfully by order and quantity, so make sure to pass the *exact number* of arguments with *exact order* as presented in `def initialize` to avoid confusion, as long as we don't want the instance's `name` to be `35` and instance's `age` to be `male`. So it is important to pass not only the exact amount of arguments but also with the exact order. 
As you might have noticed, just bellow `def initialize`, we have instance variables which store values of arguments passed, like:
`@name = name`, `@age = age`, `@gender = gender`, which command our class to keep the passed data into instance variables (`@name` is instance variable with one @, whereas `@@all` is class variable with two @@-s). After storing this data, we push all of them (or the instance with all its arguments) into the class variable `@@all`. From now on, every time we create an instance, it will be automatically stored in ` @@all` with all the passed arguments thanks to our object lifecycle presented as `def initialize(arguments)`. 
But there is one more step we need to take before our object can function, or before we can read the stored data. This is where `attr_accessor` comes into aid. As you can see, `attr_accessor` has all the same data as arguments of `def initialize`, namely, `name`, `age`, `gender`, and `attr_accessor` writes for our class both writer and reader methods, based on the value we pass.

There are two more little tricks that you can find handy, which are both helpful, and less erroneous. The first is *optional arguments*, which is written like `argument_name = nil` in case we don't want to pass any particular argument every time and, if we don't, there will be no argument error, Ruby will just omit this line. The second is *keyword arguments*, which have the following syntax `name: "name", age: "age"`, which is extremely convenient when dealing with multiple arguments when you don't want to line the arguments in the right order, just write the right value after keyword argument and avoid the necessity of keeping right order.
To conclude, the object lifecycle is both a vital and helpful part of classes, which facilitates the creation of common arguments every time a new instance is born.
