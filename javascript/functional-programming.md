# Functional Programming

## Composition [[doc]](https://medium.com/javascript-scene/composing-software-an-introduction-27b72500d6ea)
- **Function Composition**: The process of applying a function to the output of another function.

```js
const addOne = n => n + 1;
const multiplyTwo = n => n * 2;
const doublePlusTwo = x => multiplyTwo(addOne(x));
doublePlusTwo(20); // 42
```

- **Object Composition**: A composite data type is any data type which can be constructed in a program using the programming language’s primitive data types and other composite types.

    Object compositional relationships:
    - delegation
    - acquaintance: a uses-a relationship.
    - aggregation: a has-a relationship.

    > Composite objects are formed by putting objects together such that each of the latter is ‘part of’ the former.

```js
// These are primitives:
const firstName = 'Claude';
const lastName = 'Debussy';

// And this is a composite: (aggregation)
const fullName = {
    firstName,
    lastName
};
```

## Higher Order Functions
A higher order function is a function that takes one and more functions as arguments, or returns a function.