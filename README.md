# Classes Working in Collaboration

## Learning Goals

- Recognize how multiple `class`es can be used together

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

## Multiple Class Objects Reduce

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

let lotr = new Book(
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
let tolkien = new Author('J.R.R.', 'Tolkien');
let fantasy = new Genre('Fantasy');

let lotr2 = new Book('The Lord of the Rings', tolkien, fantasy, '1954');
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
values, reducing the chance for errors and keeping our data consistent.

## Sending Messages

## SWBAT 1

## SWBAT 2

## Conclusion

## Resources
