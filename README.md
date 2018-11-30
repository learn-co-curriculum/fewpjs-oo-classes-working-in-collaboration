# Classes Working in Collaboration

## Learning Goals

- Recognize how multiple `class`es can be used together

## Introduction

Using Object Oriented JavaScript, we can encapsulate closely related data and
behavior. Using `class`es, properties, methods, getters and setters, we can
finely tune how our data is accessed and modified. And since everything is
wrapped up, together, no matter where an `class` instance is needed, it behaves
in a way we've defined. A single cell in a soup of code.

With that foundation, we're now going to look at what happens when we write
multiple, related `class`es. Object Orientation is particularly effective
when it comes to representing systems of inter-related things.

In this lesson, we're going to look at some of the ways in which
`class`es can interact, and the benefits that `class` relationships can
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

let trainDreams = new Book(
	'Train Dreams',
	'Denis Johnson',
	'Historical Fiction',
	'2002'
);
```

Some of these properties might be shared between many books, though. Authors can
write many books. Many books fall into the same genres. The Object Oriented
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

Now, we can create instances for both and pass in the instances rather than
just `String`s:

```js
let denisJohnson = new Author('Denis', 'Johnson');
let historicalFiction = new Genre('Historical Fiction');

let trainDreams2 = new Book(
	'Train Dreams',
	denisJohnson,
	historicalFiction,
	'2002'
);
```

Now that we've encapsulated these related things, we can

## SWBAT 1

## SWBAT 2

## Conclusion

## Resources
