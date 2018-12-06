# Classes Working in Collaboration

## Learning Goals

- Recognize how multiple classes can share data
- Recognize how multiple classes can share behaviors
- Recognize how classes working together develop dependencies

## Introduction

Using Object Oriented JavaScript, we can encapsulate closely related data and
behaviors. Using classes, properties, methods, getters and setters, we are
able to finely tune how that data is accessed and modified. An instance of a
class carries all its behaviors with it, _even if it happens to be called from
within another class!_

In this lesson, we're going to look at one-way relationships between classes.
That is, one class using another classes data and/or behaviors.

## Recognize How Multiple Classes Can Share Data

Let's say want to keep track of a collection of books. We could write a `Book`
class, whose instances represent individual books. Each book has their own
information, title, author(s), genre, publishing date, etc...

```js
class Book {
	constructor(title, author, genre, publishingDate) {
		this.title = title;
		this.author = author;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}
}

let lordOfTheRings = new Book(
	'The Lord of the Rings',
	'J.R.R. Tolkien',
	'Fantasy',
	'1954'
);
```

Some of these properties might be shared between many books, though. For
example, an author like J.R.R. Tolkien has written many other books. Many of
those books fall into the same genre. With classes, we can encapsulate this data
as well - we can make an `Author` class that contains the name of the author:

```js
class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}
}
```

And a `Genre` class that contains the name of a genre:

```js
class Genre {
	constructor(name) {
		this.name = name;
	}
}
```

We can then create instances for both of these classes and _pass in the
instances_ rather than just `String`s when creating a `Book` instance:

```js
let tolkien = new Author('John Ronald Reuel', 'Tolkien');
let fantasy = new Genre('Fantasy');

let lordOfTheRings = new Book(
	'The Lord of the Rings',
	tolkien,
	fantasy,
	'1954'
);
```

Our new `Book` instance now contains `Genre` and `Author` instances assigned as
properties:

```js
lordOfTheRings;
// => Book {
//    title: 'The Lord of the Rings',
//    author: Author { firstName: 'John Ronald Reuel', lastName: 'Tolkien' },
//    genre: Genre { name: 'Fantasy' },
//    publishingDate: '1954' }
```

Now that we've got a set of related class instances, we can reuse them as
needed:

```js
let hobbit = new Book(
	'The Hobbit, or There and Back Again',
	tolkien,
	fantasy,
	'1937'
);

let silmarillion = new Book('The Silmarillion', tolkien, fantasy, '1977');
```

Using these instances, we've eliminated the need to repeatedly write `String`
values, reducing the chance for errors and keeping our data consistent. Because
the `tolkien` and `fantasy` values are pointing to instances of `Author` and
`Genre`, if we change the instance, all references will update as well:

```js
tolkien.firstName = 'J.R.R.';
lordOfTheRings;
// => Book {
//    title: 'The Lord of the Rings',
//    author: Author { firstName: 'J.R.R.', lastName: 'Tolkien' },
//    genre: Genre { name: 'Fantasy' },
//    publishingDate: '1954' }
lordOfTheRings.author.firstName;
// => 'J.R.R.'
```

Here, `tolkien` and `lordOfTheRings.author` refer to the same instance of
`Author`.

As we see with `lordOfTheRings.author.firstName`, by storing an instance of the
`Author` class as a property in `Book`, we gain access to properties of the
`Author` class:

```js
lordOfTheRings.author; // => Author { firstName: 'J.R.R.', lastName: 'Tolkien' }
lordOfTheRings.author.firstName; // => 'J.R.R.'
lordOfTheRings.author.lastName; // => 'Tolkien'
```

We can even write methods in one class that use the properties of another:

```js
class Book {
	constructor(title, author, genre, publishingDate) {
		this.title = title;
		this.author = author;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}

	get caption() {
		let title = this.title;
		let first = this.author.firstName;
		let last = this.author.lastName;
		let date = this.publishingDate;
		return `${title} was written by ${first} ${last} and published in ${date}.`;
	}
}
```

Here, we've created a `caption` [getter] that uses some properties of `Book`
and some of `Author`. This is the most basic way classes can work together - by
depending on each other for related data.

## Recognize How Multiple Classes Can Share Behaviors

In addition to data, classes can also utilize each other's methods. For
instance, we could add a getter to our `Author` class called `fullname`
that returns a `firstName` and `lastName` interpolated together:

```js
class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	get fullname() {
		return `${this.firstName} ${this.lastName}`;
	}
}
```

We could then rewrite our `caption` getter to use the `fullname` pseudo-property
rather than the actual properties of `Author`:

```js
class Book {
	constructor(title, author, genre, publishingDate) {
		this.title = title;
		this.author = author;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}

	get caption() {
		let title = this.title;
		let name = this.author.fullname; /* code change */
		let date = this.publishingDate;
		return `${title} was written by ${name} and published in ${date}.`; /* code change */
	}
}

lordOfTheRings.caption;
// => 'The Lord of the Rings was written by J.R.R. Tolkien and published in 1954.'
```

Here, we're using some properties from `Book`, and `fullname` from `Author`.

## Recognize How Classes Working Together Develop Dependencies

One thing to always keep in mind when working with multiple classes:
**If one class uses data or behaviors from another class, the first class
becomes _dependent_ on the second.**

In our `Book` class, for instance, we've written `caption` using
`this.author.fullname`. The `caption` getter is expecting there to be a
`fullname` property (or pseudo-property) present in `this.author`. If we
mistakenly created a `Book` instance using only `String`s again and then tried
to use `caption` we will have a problem:

```js
// A new book incorrectly created with String parameters for author and genre
let unfinishedTales = new Book(
	'Unfinished Tales',
	'J.R.R. Tolkien',
	'Fantasy',
	'1980'
);

unfinishedTales;
// => Book {
//    title: 'Unfinished Tales',
//    author: 'J.R.R. Tolkien',
//    genre: 'Fantasy',
//    publishingDate: '1954' }

unfinishedTales.author;
// => 'J.R.R. Tolkien'

// unfinishedTales.author is just a String and has no `fullname` property
unfinishedTales.author.fullname;
// => undefined

// when `fullname` is called from within `caption`, it produces undefined:
unfinishedTales.caption;
// => 'Unfinished Tales was written by undefined and published in 1980.'
```

Because `Book` relies on data and behaviors from `Author`, it has become
_dependent_ on `Author`. If we were to make changes to the `Author` class, say
we change the _name_ of a property or method, we could potentially break
functionality in `Book`.

On the flip side, if we change a property in `Book`, `Author` won't be affected.
`Author` is _not_ dependent on `Book`.

Now, if we _choose_ to reverse this relationship and have `Author` dependent
on `Book`. That might look something like the following:

```js
// notice that book is no longer keeping track of author
class Book {
	constructor(title, genre, publishingDate) {
		this.title = title;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}
}

// a new Author instance will now be given a _books property
class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
		this._books = [];
	}

	set books(books) {
		this._books = books;
	}

	get books() {
		return this._books;
	}

	get bookTitles() {
		return this._books.map(book => book.title);
	}
}

let stephenKing = new Author('Stephen', 'King');
let it = new Book('It', 'Horror', '1986');
let theShining = new Book('The Shining', 'Horror', '1977');

// the `books` setter replaces all books with whatever it is passed
// here, we pass it all the previous books plus the new book we want to add
stephenKing.books = [...stephenKing.books, it];
stephenKing.books = [...stephenKing.books, theShining];

stephenKing;
// => Author {
//     firstName: 'Stephen',
//     lastName: 'King',
//     _books: [
// 		    Book { title: 'It', genre: 'Horror', publishingDate: '1986' },
// 		    Book { title: 'The Shining', genre: 'Horror', publishingDate: '1977' }
// 	   ]
//   }

stephenKing.bookTitles;
// => ['It', 'The Shining']
```

> In this code snippet, we use the [Spread syntax][spread], `...`, to get all
> elements inside of `stephenKing.books`, and an [Array literal][literal] to
> include an additional book at the end

In the example above, the `Author` class now keeps track of `Book` instances.
`Book`s have no idea who their `Author`s are anymore. We've reversed the
relationship, and `Author` is now dependent on `Book`. Determining which class
should be dependent on others is ultimately based on the needs and purpose of
application we're building. It may also be the case that we need our `Book`s to
keep track of their `Author`s, _and_ our `Author`s to keep track of their `Book`s,
something we will touch on in upcoming lessons.

## Conclusion

We can use Object Oriented JavaScript to represent systems of related data, just
as we've seen with `Book`, `Author` and `Genre`. By storing class instances as
properties of other class instances, we can build relationships, sharing data
and behaviors. When we establish these relationships, we form dependencies
between classes.

[spread]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
[literal]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals
[getter]: https://www.google.com/search?q=getter+js&rlz=1C5CHFA_enUS757US757&oq=getter+js&aqs=chrome.0.69i59j69i60l2.1047j0j1&sourceid=chrome&ie=UTF-8
