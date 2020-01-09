---
layout: post
title:      "Simple way of complexity"
date:       2020-01-09 07:45:28 +0000
permalink:  simple_way_of_complexity
---


Collaborating objects are becoming more important and complicated as we go deeper by adding additional related classes. It's easy to understand (and even remember), that song `belongs_to :artist` and artist `has_many :songs` (underlining the importance of singular artist and plural songs), like in the example below:

```
class Song
       belongs_to :artist
end
```

```
class Artist
      has_many :songs
end
```

We can go one level deeper by associating Genre class, which, in its turn, also has many songs (just like the Artist)

```
class Genre
      has_many :songs
end
```
This time, we can say that Genre and Artist both have songs in common, but can artist also has many genres? Of course it can, but instead of just saying that Artist `has_many :genres`, we should specify their connection through the child Song, writing programmatically as Artist `has_many :genres, through: :songs`

```
class Artist
      has_many :songs
      has_many :genres, through: :songs
end
```
This is how we created connections between different classes, parent (Artist, Genre), child (Song), and something like parent-children (Genre) due to `through:` macro.
But what if, as in real life, we encounter more complex relationships, which are somehow logically connected to one another? As we plan to code, we should gather enough recourses and knowledge to thrive in real world conditions as we might face situations where these relationships are incomplete. What kind of macros we should use or create to solve all the possible ramifications of Object Relationship puzzle which weren't discussed in details before? I can think of three more to build a colorful picture.
When we think about day, we must also think about night (to predict all the possible consequences). So if we have `has_many`, we should have also `has_one`.
In order to illustrate this relationship, we could think of something like user and account number. We shouldn't say, that account `has_many` users, while this is not what the bank requirements are. Instead, we can suggest that Account `has_one :user`. 

```
class Account
      has_one :user
end
```

I'm not sure it's because of being jealous or having privacy issues, but account can only have one user, eliminating all others users' possibilities to check the money amount on that particular account. Moreover, that we can go another level deeper by proclaiming `has_one, through:`

```
class Account
      has_one :user
      has_one :user_history, through: :user
end
```
This `has_one` relationship is all about privacy (or jealousy) and sensitive data, making sure that no other perpetrator can have access to what another particular user can.
And the last (but not the least) association is all about equal rights without the necessity of intermediate assistance or, simply put, many-to-many relationship without creating a child class. It’s a mixture of `has_many` and `belongs_to`. For instance, we can have a situation in clinic where Patient receives (`has_many`) medicines, and also Medicine `has_many :patients`. Can we make a hybrid of this equal right relationship by trying to say something like (or exactly) Medicine `has_and_belongs_to_many :patients`. And, in its turn, Patient `has_and_belongs_to_many :medicines`

```
class Patient
      has_and_belongs_to_many :medicines
end

class Medicine
      has_and_belongs_to_many :patients
end
```
As my imagination is limited, I cannot think of any other possible ramifications of Object Relationship, considering that these macros are handy in real-life situations and will allow programmers to solve any related issues. But, as long as programming is constantly developing, go ahead and give it a try to think of another (or other) relationship, thus contributing in the development of information science!

