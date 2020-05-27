---
layout: post
title:      "Building bridges"
date:       2020-05-10 00:10:53 -0400
permalink:  building_bridges
---

All that I have learnt so far was like painting small separates parts of a big canvas, but without observing the picture as a whole. Only when you put all the pieces together, it burst into existence as big and fully functioning organism!
The difference between previous Rails project (apart from JS part itself) was creating resources with `--api` and, instead of having views, you render json (JavaScript Object Notation). Backend side has all the main and familiar Rails data, like MVC, with all the routes, controllers, and models (but actually without views, so you have to say MC, Models and Controllers). In controller files, you should specify ‘render json’, so you can go and grab (or fetch()) backend data nested in database.

```
class BooksController < ApplicationController
    def index
        books = Book.all
        render json: books, include: [:author, :genre], status: 200
    end
end
```

As one can see, instead of rendering view, you ‘render json’, and you ‘include: [:author , :genre]’, which points to parent object (book belongs to author and genre, author and genre have many books). After writing all the actions like index, create, destroy, show, update, having validations in models and solving CORS issue, you should delve into front-end, which consists of three major pillars: Recognizing events, Manipulating DOM, Communicating with the Server. In other words, you should grab some elements or data (targeting), add some events to the element you just grabbed (by adding addEventListeners), like `click`, `focus`, `blur`, `mouseenter`, `mouseleave` (the last two are reminding hover effect in CSS), write your methods which add functionality when firing the event, connecting to the server by whatever CRUD (create, read, update, destroy) action you desire, and sending updated data back to front-end to provide with the best user experience.
As a convention, you can store all functionality, url address, fetch requests, that provide with server-side communication, in AppAdapter.js file. For instance, you can write:

```
class AppAdapter {
    url = 'http://localhost:3000';
    static domElements = {
        newBookForm: document.getElementById('new-book-form')
    };

    bindEventListeners() {
        this.constructor.domElements.newBookForm.addEventListener('submit', () => this.createBook(event));
    }


    getBooks() {
        fetch(this.url + '/books')
            .then((response) => response.json())
            .then((data) => {
                data.forEach((book) => {
           const { id, title, publisher, rating, author, genre } = book;
           new Books(id, title, publisher, rating, author, genre);   if (!AppContainer.authors.map((author) => author.name).includes(book.author.name)) {
          new Authors(book.author.name, book.author.gender, book.author.age, book.author.email);
                    }
                });
                AppContainer.showBooks();             })
               .catch((err) => console.log(err));
    }

    createBook(event) {
        event.preventDefault();
        fetch(`${this.url}/books`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                Accept: 'application/json'
            },
            body: JSON.stringify({
                title: event.target.title.value,
                publisher: event.target.publisher.value,
                rating: event.target.rating.value
            })
        })
            .then((resp) => resp.json())
            .then((data) => {
            const { id, title, publisher, rating, author, genre } = data;
            new Books(id, title, publisher, rating, author, genre);
            })
            .catch((err) => console.log(err));
    }
	}
```

The best representation of AJAX (asynchronous JavaScript and XML) is fetch() request, which renders data from the server asynchronously (**not** waiting one response to be completed to trigger another one) and **without** necessity of reloading the page. As users don't like to wait for one page to answer more than several seconds, we provide them with HTML and CSS before loading JS so that they don't surf further because of their impatience. 
Fetch method:
1.sends GET request (unless indicated otherwise) to the domain address, where data exists (for instance, ‘http://localhost:3000/books’). If we want to send other types of requests, such as POST, PUT, PATCH or DELETE, we should specify right after page address, like so:
```
fetch(`${this.url }/books`, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json'
			},
			body: JSON.stringify({
				title: event.target.title.value,
				publisher: event.target.publisher.value,
				rating: event.target.rating.value
			})
		})
```
2. Then server returns Promise, which indicates whether our asynchronous request was successful or with failure (answering with 3 states: *pending, fulfilled, rejected*). It is like a placeholder before our actual data arrives (we should remember, that Promise is not the actual content we are requesting itself!). We convert the response recieved by `.then(response => response.json())` method, which takes this response string and converts it via `json` object, so that it is browser-readable, `.then(data => console.log(data))` method (or any other conventional name for "data"), which represents a giant object with key: value pairs. You can guess right away (or simply check), that this data has all the parameters you have written in back-end’s Books table, like ‘id’, ‘title’, ‘rating’, etc, and thus we can finally work with the returned data and, for example, create a new instance of Books class or show them in the browser. 
3. While `response.json()` turns string into object, `body: JSON.stringify()` does just the opposite. It, as the method name suggests, turns object into a string before sending to the server (during POST, PUT and PATCH requests).
4. You console log errors (if any) and both create and get instances of books table. Creating a new instance takes to a new js file page (book.js) where every time, when a new instance is created, it is stored in AppContainer.books array (another AppContainer.js file!)

```
class Books {
    constructor(id, title, publisher, rating, author, genre) {
        this.id = id;
        this.title = title;
        this.publisher = publisher;
        this.rating = rating;
        this.author = author;
        this.genre = genre;
        AppContainer.books.push(this); 
    }
}
```

You have access to your AppContainer books array across all your files by declaring it to be ‘static’, like in example below:

```
class AppContainer {
    static authors = []; 
    static books = [];
    static genres = []
}
```

‘Static’ is like class variable in Ruby (@@i_am_class_variable) which answers to the class and not to the instance. It is very handful when we create an array or method somewhere, then decide to call it from elsewhere (visible across all files, as you said before).

Adding Event Listeners.

Adding an event is the second part of DOM manipulation (targeting, adding event listeners, performing functionality (maybe this is where you should coin a new abbreviation, ‘TAF’, which would stand for ‘target’, ‘add’, ‘function’??)). Targeting of HTML elements can be done in many different ways, by getting element by id (singular), getting elements by class or tag names (plurar), query selector, query selector all, etc. We can write in AppAdapter:

```
const newBookForm = document.getElementById('new-book-form');
newBookForm.addEventListener('submit', () => this.createBook(event));
```

First line, as mentioned, is targeting, second one is adding an event and invoking function (it’s not the function itself, but its name for invocation purpose). If the reader is not too lazy reading several lines above in AppAdapter , he/she can see what the `createBooks()` function looks like. Long story short, it gets all the user input data of HTML form, prevents default behavior (by adding `event.preventDafault()`), sends it to back-end via `fetch()`, with ‘post’ method (you  should create both route and controller create action), updates Books object with `JSON.stringify()` method, creates new Book instance, pushes it into books array, updates DOM tree with `fetch()` get and, voila, your newly created book is visible in your HTML page (what a simple, but essential work you did!). 
All this full-stack tough journey is about connecting dots, building bridges, throwing away and catching back (or posting and getting? ), communicating, painting. I am stuck of creating any more metaphors! But, most importantly, this is the crossing line, where “software” and “engineering” words converge, receive a brand new meaning and paramount value as you finally understand, what it means to engineer a software!

