# Javascript Note (ECMAScript 6)

## [`this` Keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
It's always a reference to an object (auto-boxing if it's not an object) in non-strict mode, but in strict mode it can be any value.
- Used outside any function: refer to the global object (`window` in web).
- Used in strict mode: It will be undefined unless being assigned
- Used inside a function: the value of this depends on how the function is called
	- default to the global object (not strict mode)
	- an object method: refer to the object
	- called by `Function.prototype.apply()` or `Function.prototype.call()` or `Function.prototype.bind()`: refer to the first argument pass to the API
	- an arrow function: `this` retains the value of enclosing lexical context's `this`
	- on the object's prototype chain: refer to the object the method was called on
	- a DOM event handler: refer to the DOM element which the listener is placed
- Used in an inline event handler: refer to the DOM element
- Used in classes similar to used in function except:
	- a constructor: refer to the new object being constructed
	- a derived class(`extend` another class) constructor: use `this` before calling `super()` will throw an error, otherwise `this` will refer to the inherited class
	- classes are always strict mode code (`this` defaults to `undefined`)

## Arrow Function Expression
### Caveats
- Don't have their own `arguments` and `this`, so they retain the values from enclosing lexical context
	```js
	function foo() {
	  setTimeout(() => {
	    console.log('id:', this.id);
	  }, 100);
	}

	var id = 21;

	foo.call({ id: 42 });
	// id: 42
	```
	```js
	var arguments = [1, 2, 3];
	var arr = () => arguments[0];

	arr(); // 1

	function foo(n) {
	  var f = () => arguments[0] + n; // foo's implicit arguments binding. arguments[0] is n
	  return f();
	}

	foo(3); // 6
	```
- Can't be used as a constructor
- Don't have a `prototype` property
- Can't use `yield` keyword, and hence can't be used as a generator function