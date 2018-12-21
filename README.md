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
	'Literature',
	'1954'
);
```

Some of these properties might be shared between many books, though. For
example, J.R.R. Tolkien also wrote 'The Hobbit' and 'The Silmarillion.' All
three books fall into the same _genre_. With classes, we can encapsulate
concepts like a _genre_ and an _author_ and use them in collaboration with our
`Book` class!

First, we'll make an `Author` class that contains the name of the author:

```js
class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}
}
```

And then a `Genre` class that contains the type of a genre:

```js
class Genre {
	constructor(type) {
		this.type = type;
	}
}
```

We can then create instances for both of these classes and _pass in the
instances_ when creating a `Book` instance rather than just using `String`s:

```js
let tolkien = new Author('John Ronald Reuel', 'Tolkien');
let literature = new Genre('Literature');

let lordOfTheRings = new Book(
	'The Lord of the Rings',
	tolkien,
	literature,
	'1954'
);

lordOfTheRings;
// => Book {
//    title: 'The Lord of the Rings',
//    author: Author { firstName: 'John Ronald Reuel', lastName: 'Tolkien' },
//    genre: Genre { type: 'Literature' },
//    publishingDate: '1954' }
```

Our new `Book` instance now contains `Genre` and `Author` instances assigned as
properties.

Creating classes for these concepts increases our ability to reuse code:

```js
let hobbit = new Book(
	'The Hobbit, or There and Back Again',
	tolkien,
	literature,
	'1937'
);

let silmarillion = new Book('The Silmarillion', tolkien, literature, '1977');
```

Using these instances, we've eliminated the need to repeatedly write `String`
values, reducing the chance for errors and keeping our data consistent. Because
the `tolkien` and `literature` values are pointing to instances of `Author` and
`Genre`, if we change the instance, all references will update as well:

```js
tolkien.firstName = 'J.R.R.';
lordOfTheRings;
// => Book {
//    title: 'The Lord of the Rings',
//    author: Author { firstName: 'J.R.R.', lastName: 'Tolkien' },
//    genre: Genre { type: 'Literature' },
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
class Genre {
	constructor(type) {
		this.type = type;
	}
}

class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}
}

class Book {
	constructor(title, author, genre, publishingDate) {
		this.title = title;
		this.author = author;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}

	// the caption method accesses data from this.author
	get caption() {
		let title = this.title;
		let first = this.author.firstName;
		let last = this.author.lastName;
		let date = this.publishingDate;
		return `${title} was written by ${first} ${last} and published in ${date}.`;
	}
}

let science = new Genre('Science');
let stephen = new Author('Stephen', 'Hawking');
let universeInNutshell = new Book(
	'The Universe in a Nutshell',
	stephen,
	science,
	'2001'
);

universeInNutshell.caption;
// => 'The Universe in a Nutshell was written by Stephen Hawking and published in 2001.'
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

Then rewrite our `caption` getter to use the `fullname` pseudo-property
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

stephen.caption;
// => 'The Universe in a Nutshell was written by Stephen Hawking and published in 2001.'
```

Now, we're using some properties from `Book`, and `fullname` from `Author`.

## Recognize How Classes Working Together Develop Dependencies

One thing to always keep in mind when working with multiple classes:
**If one class uses data or behaviors from another class, the first class
becomes _dependent_ on the second.**

In our `Book` class, for instance, we've written `caption` using
`this.author.fullname`. The `caption` getter is expecting there to be a
`fullname` property (or pseudo-property) present in `this.author`.

Because `Book` relies on data and behaviors from `Author`, it has become
_dependent_ on `Author` to function .

If we were to make changes to the `Author` class, say
we change the _name_ of a property or method, we could potentially break
functionality in `Book`. The same applies to `Book` and `Genre`. If we were
to visually represent these dependencies, it would look something like this:

<img src="https://curriculum-content.s3.amazonaws.com/fewpjs/fewpjs-class-relationships/author_book_genre.png" width= 50% alt="image representing dependencies"/>

The arrows can be thought of as the direction of the relationship - `Book`
_knows_ about `Author` and `Genre`; `Book` is dependent upon them. We can access
instances of authors and genres _through_ books.

An `Author` or `Genre`, on the other hand, does not know about `Book`. If we change a property in `Book`, `Author` and `Genre` won't be affected.
`Author` and `Genre` are _not_ dependent on `Book`.

## Conclusion

We can use Object Oriented JavaScript to represent systems of related data, just
as we've seen with `Book`, `Author` and `Genre`. By storing class instances as
properties of other class instances, we can build relationships, sharing data
and behaviors. When we establish these relationships, we form dependencies
between classes.

[spread]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
[literal]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals
[getter]: https://www.google.com/search?q=getter+js&rlz=1C5CHFA_enUS757US757&oq=getter+js&aqs=chrome.0.69i59j69i60l2.1047j0j1&sourceid=chrome&ie=UTF-8
[dewey]: https://www.youtube.com/watch?v=RNEDEk1Tz4Y
