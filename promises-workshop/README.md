# Train your skill in following the flow of data while understanding the concept of promises

## The flow of control and the flow of data

In this workshop, you'll build on the skills you learnt in the [flow of control (callbacks) workshop](../callbacks-workshop/).  Following the flow of data supports and is supported by following the flow of control.

## Why promises

* More powerful function and error composition in asynchronous code.  (Research topic!)

* Clarify "callback hell" by replacing it with straight-line code.  For example:

```js
getPeople()
  .then(first)
  .then(extractId)
  .then(getPerson)
  .then(extractFavouriteMusic)
  .then(cheerForFavouriteMusic);
```

## Abilities that this workshop focuses on

This workshop won't cover why promises are useful.  It will cover how they work.

In this workshop you'll improve your ability to:

1. Describe "the data flow of a program" as "the way that data moves through a program".
2. Explain how to follow the flow of data through a program.
3. Follow the flow of data to help you understand promises.

Is the purpose and subject of this workshop clear?

### Process for understanding the flow of data

This process layers on top of the process for understanding the flow of control.  See the [callbacks worksheet](../callbacks-workshop/README.md) for a reminder of that process.

1. Follow the flow of control.

2. `console.log` *variables* and *function return values* to see their data.  Use more `console.log`s to follow this data as it flows on through the rest of your program.

## Workshop (1 hour)

### Thumbs (1 min)

How confident are you in each of the three abilities above?

By the end of the workshop, your thumbs will hopefully be a few notches higher.

### Demo (15 mins)

#### Demo 1: print some people

##### With a callback

```js
$.get("https://async-workshops-api.herokuapp.com/people", function(people) {
  console.log(people);
});
```

###### With a promise

```js
$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    console.log(people);
  });
```

Things to maybe `console.log` to get a better understanding of this code:

* The return value of `get`.

#### Demo 2: print a person's favourite music

##### With callbacks

```js
$.get("https://async-workshops-api.herokuapp.com/people", function(people) {
  $.get("https://async-workshops-api.herokuapp.com/people/" + people[0].id, function(person) {
    console.log(person.favouriteMusic);
  });
});
```

##### With promises

```js
$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    return $.get("https://async-workshops-api.herokuapp.com/people/" + people[0].id);
  })
  .then(function(person) {
    console.log(person.favouriteMusic);
  });
```

Things to maybe `console.log` to get a better understanding of this code:

* `people`
* `person`
* The return value of the first `then` function.
* The return value of the second `then` function.

### Demo 3: a broken promise

```js
$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    people[0].id;
  })
  .then(function(personId) {
    $.get("https://async-workshops-api.herokuapp.com/people/" + personId);
  });
  .then(function(person) {
    console.log(person.favouriteMusic);
  });
```

What should I `console.log` to help me fix this code?

### Thumbs (1 min)

How confident are you in each of the three abilities we talked about at the beginning of the workshop?

(Hopefully thumbs have gone up for first two items.  If not, maybe try diagramming?)

### Setup (5 mins)

Clone the repo to your computer, run `npm install` and open `promises-workshop/index.html` in your web browser.  It should say `hi!` in your browser developer console.  [More help](../run-the-question-code.md).

### Workshop (30 mins)

* Pair up.  Choose who will drive and who will navigate.  This will be a great opportunity to practice maintaining the driver/navigator roles during investigatory coding.  Remember the XP values of communication, feedback, respect, courage and simplicity.

* Put the code for question 1 (below) into `promises-workshop/index.js`.  Open `index.html` in your browser.  [More help](../run-the-question-code.md).

* Follow the process outlined in the demo to understand the flow of control of the code in the questions.

* Swap driver and navigator and move onto question 2.

### Plenary (15 mins)

We'll come back together, show and analyse our code, and answer questions.

### Thumbs (1 min)

How confident are you in each of the three abilities we began the workshop with?

### After the workshop

* A developer constantly analyses the flow of data in their code.  Keep trying to improve this skill.

* The more you can correctly read the flow of data without running the code, the faster you'll be.  Build this intuition by making predictions and checking if your prediction is right.

* To understand more about promises, and to get more practice following the flow of data, complete the questions you didn't have time to do in the workshop.

## Questions

### Question 1

What does this code print?

How are the data collated to produce the final printed value?

```js
$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    return $.get("https://async-workshops-api.herokuapp.com/people/" + people[0].id);
  })
  .then(function(person) {
    return [person.favouriteMusic];
  })
  .then(function(favouriteMusic) {
    return favouriteMusic.concat("Bob Dylan");
  })
  .then(function(favouriteMusic) {
    return favouriteMusic.concat("Sonic Youth");
  })
  .then(function(favouriteMusic) {
    console.log(favouriteMusic);
  });
```

### Question 2

Before running the code below, predict what it will print out.

```js
var cheer = "Wooo";

$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    return $.get("https://async-workshops-api.herokuapp.com/people/" + people[0].id);
  })
  .then(function(person) {
    console.log(cheer + " " + person.favouriteMusic);
  });

console.log(cheer + " Bob Dylan");
```

### Question 3

Try rewriting this code using callbacks.  Which is easier to read - the callbacks version or the promises version? Why?

Terminology: first class functions, functional programming, functional composition.

```js
function first(array) {
  return array[0];
};

function extractId(person) {
  return person.id;
};

function extractFavouriteMusic(person) {
  return person.favouriteMusic;
};

function cheerForFavouriteMusic(music) {
  console.log("Wooo " + music);
};

function getPerson(id) {
  return $.get("https://async-workshops-api.herokuapp.com/people/" + id);
};

function getPeople() {
  return $.get("https://async-workshops-api.herokuapp.com/people");
};

getPeople()
  .then(first)
  .then(extractId)
  .then(getPerson)
  .then(extractFavouriteMusic)
  .then(cheerForFavouriteMusic);
```

### Question 4

How does the value in `people[0].id` become a value that allows `then` to be called? (Research topic!)

```js
$.get("https://async-workshops-api.herokuapp.com/people")
  .then(function(people) {
    return people[0].id;
  })
  .then(function(personId) {
    return $.get("https://async-workshops-api.herokuapp.com/people/" + personId);
  })
  .then(function(person) {
    console.log(person.favouriteMusic);
  });
```

### Question 5

Inspect the data in this program to figure out how it works.

```js
function after(delay) {
  return new Promise(function(resolve) {
    setTimeout(resolve, delay);
  });
};

after(1000)
  .then(function() {
    console.log("Surprise!");
  })
```

### Question 6

Write a program that creates a promise that sometimes fulfills and sometimes rejects.  The code that uses the promise should handle both cases.

## Resources

* [Promises (Mozilla)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* :pill: [Promises](https://github.com/makersacademy/course/blob/master/pills/js_promises.md)
