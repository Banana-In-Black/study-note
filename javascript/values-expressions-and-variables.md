# Values, Expressions And Variables

## Values
There're two kinds of values, both can be checking by `typeof(value)` or `typeof value`.

### Primitive Values (Immutable)
- Undefined: `typeof(undefined) === 'undefined'`.
- Null: `typeof(null) === 'object'`. ([historical issue](https://stackoverflow.com/questions/18808226/why-is-typeof-null-object))
- Numbers: `typeof(12.3) === 'number'`.
- Strings: `typeof('hello') === 'string'`.
- Booleans: `typeof(true) === 'boolean'`.
- Symbols: `typeof(Symbol('hi')) === 'symbol'`.
- BigInts: `typeof(BigInt(45)) === 'bigint'`.

#### Immutable
Trying to modify primitive values will silently be refused or result in errors depends on which mode (strict mode or not) your code is running in.
```js
let str = "yikes";
str[0] = 'l';
console.log(str); // yikes

let fifty = 50;
fifty.shades = 'gray';
console.log(fifty.shades); // undefined

'use strict';
fifty.no = 456; // TypeError: Cannot create property 'no' on number '50'
```

### Object and Functions (Mutable)
- Objects: `typeof({}) === 'object'`.
- Function: `typeof(x => x) === 'function'`.

## Expressions
An expression evaluates to a value.
```js
2 + 2             // express a mathematical result
[1, 2, 3].slice() // express a function call result
/hello(world)/    // a regular expression
```

### The Difference from Statement
A statement describe a behavior (do something), it's more like a sentence.
```js
let fullName = `${firstName} ${lastName}`;
```

## Variables
A variable is a reference to a value.

```js
let pet = 'Narwhal'; // pet ---> 'Narwhal'
pet = 'The Kraken';  // pet ---> 'The Kraken'
console.log(pet);    // The Kraken
```

```js
let x = 10;     // x ---> 10
let y = x;      // y ---> 10, y is a reference copy.
x = 0;          // x ---> 0
                // y ---> 10
console.log(y); // 10
```

### Passing Variables to Funcation Call
```js
let primitive = 'abc';
(function modify(v) {
    v.test = 123; // no effect because it's immutable
    v = 'cde';    // v ---> 'cde'
                  // primitive ---> 'abc'
}(primitive));
console.log(primitive); // abc
```
```js
let nonPrimitive = [1, 2, 3];
(function modify(v) {
    v.push(4);
}(nonPrimitive));
console.log(nonPrimitive); // [1, 2, 3, 4]
```