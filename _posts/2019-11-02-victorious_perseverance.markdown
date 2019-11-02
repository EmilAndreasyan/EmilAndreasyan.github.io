---
layout: post
title:      "Victorious perseverance"
date:       2019-11-02 04:15:03 +0000
permalink:  victorious_perseverance
---


It is difficult to start writing this blog post, but, as soon as the start is given, everything should work out perfectly. This was my first thought and the exact situation with my first project. I had some knowledge about all the concepts that we have covered so far, but I didn’t know how to start the project, how to put everything together, I even spent hours trying to run my terminal and set it up in my text editor. It was a nightmare trying to attach everything together (one should always remember about `require` and `require_relative` here), avoiding errors and shooting troubles out, but eventually I was able to write that “hello world” in the terminal! From that moment on, I was ready to go to the battle.
I decided to scrape a random site from where I could store and manipulate some data the users might be interested in, and thus I picked a site called TubiTv (perhaps because I am a movie aficionado). All I knew, that my program should consist of three main parts, namely TubiTv::Scraper, TubiTv::Movies, TubiTv::CLI. The first one is for scraping data from the outside world, the second one is for storing it all in `@@all`, and CLI to manipulate with the data I had in my possession.
This is how my Scraper looked like:

```
class TubiTv::Scraper
    
    def self.scrape_movies
        @site = "https://tubitv.com/category/most_popular?tracked=1"
        @doc = Nokogiri::HTML(open(@site))
        @container = @doc.css(".Col.Col--4.Col--lg-3.Col--xl-1-5.Col--xxl-2")
        @container.each do |movie|
        title = movie.css(".JB9zq h3").text
        duration = movie.css(".yPcUu").text
        year = movie.css("._3BhXb").text
        genre = movie.css(".RmVOo").text
        movies_hash = {
            :title => title,
            :duration => duration,
            :year => year,
            :genre => genre,
        }
        TubiTv::Movies.new(movies_hash)
    end
    end
end
```

It would iterate over the data, and save its parts in different variables, so I could initialize an instance of `TubiTv::Movie` class. As a convenience, I stored that data in a hash so it’s more easily implemented in `Movie` class.
In the above-mentioned class, not only the `movie_hash` is to be put in place of the argument so the overall class could be more abstract, but also ‘each’ and ‘send’ methods are used, so less could be written and more could be done, if I ever want to get back to the code and add some other data variable (all I have to do here is to adjust `attr_accessor`, by adding or deleting one of its values, without changing anything in `initialize` method).

```
class TubiTv::Movies
    attr_accessor :title, :duration, :year, :genre

    @@all = []
    def initialize(movies_hash)
        movies_hash.each do |key, value|
            self.send("#{key}=", value)
        end
        descriptions = []
        @@all << self
    end

    def self.all
        @@all.uniq
    end
end
```

Well, two parts of the project are set up, but I should write the third, the most challenging one.
First of all, I need to find out how to scrape, instantiate and store all the data in my CLI so I could work with it, and thus I wrote a method called `get_movies`:

```
def get_movies:
        TubiTv::Scraper.scrape_movies # calls scrape_movies method from Scraper
        @movies = TubiTv::Movies.all #  retrieves scraped data from Movies.all and stores it in @movies instance variable
end
```

This method does three vital things: scrapes all the required data, stores them in `Movies.all` class variable, then transfers these data to `@movies` instance variable that was created (it may be called anything, but `@movies` is a meaningful name for a variable so anyone can understand what type of data it consists of). From now on, all the manipulation and magic could be created from it. It is important to underline, that it must be an instance, and not scope variable, so it could be used throughout the Class.
So it seemed impossible initially, but wasn’t so hard after all. The hardest part is yet to come, to write all the logic and hope that your code would be error-proof. This is a challenging part of the ability to think logically and gradually. This is where you should write iterations like `each`, `select`, set conditionals like `if` and `else`, question the code with Boolean methods like `empty?`, `all?` or `include?`, `split`-ing,  `join`-ing and RegEx-ing to select that particular part of the string, and this is exactly where one realizes, how both easy and powerful Ruby could be. After all the hours spent in my text editor and with the essential help from my tutor Juan Castillo, I was eventually able to provide the imaginary user with several options like:

		puts "if you want to list all the movies, type 1"
		puts "if you want to filter movies by year, type 2"
		puts "if you want to filter movies by year range, type 3"
		puts "if you want to filter movies by genre, type 4"
		puts "if you want to filter movies by both genre and year, type 5"
		puts "if you want to find movies by its title, type 6"


To my total astonishment, all methods worked perfectly and seamlessly. For instance, if a user presses `5`, it will hint him with following message:

`puts "Enter the year of a movie you're looking for".colorize(:cyan)`

After accepting user’s year as integer (or converting it to integer with `to_i` method), it will give the second hint:

`puts "Enter the genre".colorize(:cyan)`

After typing both year and genre, it starts iterating over all the data stored in `@movies`, and prints out only those meeting both criteria. Isn’t it wonderful?! And, you might guess, all other methods work as effectively and effortlessly!
This very first project taught me many things, from setting the environment up till writing and running sophisticated methods, but in order to outline only one concept having the highest importance, I would say: never give up, while it’s definitely worth it, or, in other words, “perseverance till the ultimate victory is achieved”!
So, when can I start my next project?

