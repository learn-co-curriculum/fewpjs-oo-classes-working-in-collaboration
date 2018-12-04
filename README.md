# Classes Working in Collaboration

## Learning Goals

- Recognize how multiple `class`es can share data
- Recognize how multiple `class`es can share behaviors
- Recognize how `class`es working together develop dependencies

## Introduction

Using Object Oriented JavaScript, we can encapsulate closely related data and
behaviors. Using `class`es, properties, methods, getters and setters, we are
able to finely tune how our data is accessed and modified. And since everything
is wrapped up together, no matter where an `class` instance is needed, it
behaves in the way we've defined.

With that foundation, we're now going to look at what happens when we write
multiple, related `class`es. Object Orientation is particularly effective
when it comes to representing systems of interrelated things.

In this lesson, we're going to look at some of the ways in which
`class`es can interact and the benefits that `class` relationships can
provide us.

## Sharing Data between Multiple Classes

Let's say we were designing an app to help search a public library's catalogue,
that is, all the books it has available. We could write a `Book` `class`, whose
instances represent individual books. Each book has their own information,
title, author(s), genre, publishing date, etc...

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

Some of these properties might be shared between many books, though. Tolkien has
written other books. Many books fall into the same genre. The Object Oriented
solution would be to simply encapsulate this data as well - we can make an
`Author` `class` that contains the name of the author:

```js
class Author {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}
}
```

And a `Genre` `class` that contains the name of a genre:

```js
class Genre {
	constructor(name) {
		this.name = name;
	}
}
```

We can then create instances for both of these `class`es and _pass in the
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

Now that we've got a set of related `class` instances, we can reuse them as
needed:

```js
let hobbit = new Book(
	'The Hobbit, or There and Back Again',
	tolkien,
	fantasy,
	'1937'
);
```

```js
let silmarillion = new Book('The Silmarillion', tolkien, fantasy, '1977');
```

Using these instances, we've eliminated the need to repeatedly write `String`
values, reducing the chance for errors and keeping our data consistent. Because
the `tolkien` and `fantasy` values are pointing to instances, if we change
the instance, all references will update as well:

```js
tolkien.firstName = 'J.R.R.';
lordOfTheRings;
// => Book {
//    title: 'The Lord of the Rings',
//    author: Author { firstName: 'J.R.R.', lastName: 'Tolkien' },
//    genre: Genre { name: 'Fantasy' },
//    publishingDate: '1954' }
```

Another important benefit we gain is that we can have a `class` use its _own_
property to access _another_ `class`es data:

```js
lordOfTheRings.author; // => Author { firstName: 'J.R.R.', lastName: 'Tolkien' }
lordOfTheRings.author.lastName; // => 'Tolkien'
```

This is the most basic way `class`es can work together - by depending on each other
to store related data.

## Sharing Behavior between Multiple Classes

In addition to data, `class`es can also utilize each other's methods. For
instance, we could add a getter to our `Author` class called `fullname`:

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

...and then utilize this in `Book`:

```js
class Book {
	constructor(title, author, genre, publishingDate) {
		this.title = title;
		this.author = author;
		this.genre = genre;
		this.publishingDate = publishingDate;
	}

	get infoStatement() {
		return `${this.title} was written by ${
			this.author.fullname
		} and published in ${this.publishingDate}.`;
	}
}

lordOfTheRings.infoStatement;
// => 'The Lord of the Rings was written by J.R.R. Tolkien and published in 1954.'
```

Here, we're using properties of the `Book` `class` and `fullname` from the
`Author` `class` together to form a sentence.

## Sharing Data and Behaviors Creates Dependencies between Classes

One thing to always keep in mind when working with multiple `class`es:
If one `class` uses data or behaviors from another `class`, the first `class`
becomes _dependent_ on the second.

In our `Book` `class`, for instance, we've written `infoStatement` using
`this.author.fullname`. The `infoStatement` getter is expecting there to be a
`fullname` property (or pseudo-property) present in `this.author`. If we
mistakenly created a `Book` instance using only `String`s again and then tried
to use `infoStatement` we will have a problem:

```js
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
unfinishedTales.author.fullname;
// => undefined
unfinishedTales.infoStatement;
// => 'Unfinished Tales was written by undefined and published in 1980.'
```

Because of what we have written, `Book` is has become dependent on `Author` in
order to fully function correctly. In a way, `Book` has become less flexible
due to this dependency and we will revisit this idea later. For now, though,
these dependencies can be very useful to recognize. They are core to `class`es
working in collaboration.

## Using Complex Dependencies

So far, we've seen

dependencies

## Designing Collaboration

## Sending Messages

## SWBAT 1

## SWBAT 2

## Conclusion

## Resources
