---
layout: post
title:      "Going in circles"
date:       2019-10-22 04:13:02 +0000
permalink:  going_in_circles
---

Loops, together with Iteration, may not be one of the most important and handy tools in programming. Any code without looping (where it is applicable) would be impossible or, at least, a nightmare in programming, requiring many lines of unnecessary code, undermining abstraction and making our code difficult to write and to read.  But first of all, we are to differentiate looping from iteration. Iteration requires an array and runs over its arguments, while for looping a programmer must set conditions like: do anything for certain time; do anything while something is true, don't do anything unless something is true. There are various types of looping related to the task we would like to complete. Here are some examples:

A simple loop:

```
loop do
puts "infinite loop" #body
end
```

As you can suggest from the `puts` method, we should avoid such type of looping while our computer memory it imminently run out of space. We don't want that to happen, do we? We can control our loop by setting start and end, preventing it to run without purpose and infinitely. In this controlled loop we should initiate starting and ending points (integers) the loop should iterate within:

```
count = 0 # to begin with
loop do
count +=1 # to increase by 1 after each execution
puts count
break
end
```

Let's see what happened here! First of all, we set a variable called `count` to integer 0 outside of the loop. Then, inside of it, we increased this `count` variable value by 1 after each cycle (actually, we could increase steps by writing something like count += 5, which will increase the value by 5 after each cycle). But wait, what does += equation stand for? `count += 1` equals to `count = count + 1`, which means literally, that program will add 1 to the `count` variable each time after execution, gradually increasing it to the point when we command the program to stop. Then we ask Ruby to write down the value of `count`, by declaring  `puts count`. And last, but not the least, we command the program to `break` so that the loop doesn't fill the universe of the program by running infinetely. But, as we see, there is very little use for the loop to break after first cycle. So, how can we say Ruby to run for certain times *while* some condition is true?

```
count = 0 # to begin with
while count <= 5 # condition
puts count # execution
count += 1 # to increase by 1 after each execution
end
```

In this code, if we try to convert it to common language, it would sound something like this: start the count with 0, increase it while the count is less than or equal to 5, print count value after each looping.
 We noticed that initially we wrote count = 0, then, at the end, count +=1. *Both* of these 2 lines of code are required for the loop to run,  by *setting* initial value, and *increasing* it `while` some condition is `true` (`while count <= 5`). Otherwise, it will never reach 5 and meet the condition.

You might think, that if we set our program to do something while some condition is met, can we also require it to do something opposite - do not execute until some condition is met? In other words, `while` works when condition is `true`, whereas `until` runs as long as condition is `false`. Turns out we can ask Ruby to do it by the following way:

```
until count < 0
  puts count
  count -= 1
end
```

Further we can enhance our loop by adding some more functionality, or by setting a condition. , What if we want to perform something if certain condition is true? for instance, what if we want to print only odd or even numbers? Let's elaborate in the following code: 

```
count = 0

while xcount<= 10
  if count.even?	
    puts count
  end
  count += 1
end
```

This one is more sophisticated as we declared to print count out only if that integer is even, by writing `if count.even?` which will execute each time, when this condition is `true`.

As one can suggest from the following examples, Loops are both necessity and luxury which helps solve many issues.


