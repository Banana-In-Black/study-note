# Functional Programming
[[doc]](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

Functional programming (often abbreviated **FP**) is the process of building software by composing **pure functions**, avoiding **shared state**, **mutable data**, and **side-effects**. Functional programming is **declarative** rather than **imperative**, and application state flows through pure functions. Contrast with object oriented programming, where application state is usually shared and colocated with methods in objects.

## Composition [[doc]](https://medium.com/javascript-scene/composing-software-an-introduction-27b72500d6ea)
- **Function Composition**: The process of combining two or more functions to produce a new function. Composing functions together is like snapping together a series of pipes for our data to flow through.

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

## Pure Functions [[doc]](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)
### Definition
- Given the same input, will always return the same output. (predictable)
- Produces no side effects. (independent)
    * Must not mutate arguments or external state. (immutability)
    * Must not call any impure functions.

> A dead giveaway that a function is impure is if it makes sense to call it without using its return value. For pure functions, that’s a *noop*.

```js
// Assume array items are primitive values only
function pureAddToArray(ary, item) {
    return [...ary, item]; // shallow copy
}

function impureAddToArray(ary, item) {
    return ary.push(item); // ary is mutated
}
```

## Higher Order Functions
A higher order function is a function that takes one and more functions as arguments, or returns a function.

## Curry vs. Partial Application [[doc]](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8)
### Definition
- **Partial Application**: The process of applying a function to some of its arguments. (Transform a n-ary function into a m-ary function, n > m)
- **Curry**: Convert a single function of n arguments into n functions with a single argument each. (Transform a n-ary function into n * unary functions)

The most common case of curry is function composition.

```js
import { curry, compose } from 'lodash-es';

const map = curry((fn, array) => array.map(fn)); // curried map
const double = n => n * 2;
const addOne = n => n + 1;
const doubleAll = map(double); // (array) => array
const addOneAll = map(addOne); // (array) => array
const doubleAndPlusOneAll = compose(addOneAll, doubleAll); // (array) => array

const numArray = [1, 2, 3, 4];
doubleAndPlusOneAll(numArray); // [3, 5, 7, 9]
```