# Javascript Note (ECMAScript 6)

## `this` Keyword [[doc]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
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

## Prototype chain [[doc1]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) [[doc2]](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype.

- An object's prototype is available via `Object.getPrototypeOf(obj)`, or via the deprecated `__proto__` property.
- The `prototype` property on constructor functions is automatically created while the constructor function was declared.
- The former is the property on each instance, and the latter is the property on the constructor.

```js
function MyConstructor() {}
MyConstructor.prototype; // MyConstructor { constructor: f }
MyConstructor.prototype.constructor === MyConstructor;  // true

var obj = new MyConstructor();
obj.prototype; // undefined

Object.getPrototypeOf(obj) === obj.prototype; // false
Object.getPrototypeOf(obj) === MyConstructor.prototype; // true
Object.getPrototypeOf(obj) === obj.__proto__; // true

Object.getPrototypeOf(MyConstructor) === Function.prototype; // true
```

### Inheritance
- `Object.create(proto[, propertiesObject])` [[doc]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

	```js
	// Parent class
	function Shape() {
	this.x = 0;
	this.y = 0;
	}

	Shape.prototype.move = function(x, y) {
	this.x += x;
	this.y += y;
	console.info('Shape moved.');
	};

	// Child class
	function Rectangle() {
	Shape.call(this); // call super constructor.
	}

	// Inheritance
	Rectangle.prototype = Object.create(Shape.prototype);
	Rectangle.prototype.constructor = Rectangle;

	var rect = new Rectangle();

	console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle);// true
	console.log('Is rect an instance of Shape?', rect instanceof Shape);// true
	rect.move(1, 1); // Outputs, 'Shape moved.'
	```

	- Modify `prototype.constructor` won't have an effect while you initiate an object (by using `new`). It's only useful while you need a constructor reference from instance. [[stackoverflow]](https://stackoverflow.com/questions/9267157/why-is-it-impossible-to-change-constructor-function-from-prototype)
	- The difference between
		```js
		Child.prototype = Object.create(Parent.prototype);
		```
		and
		```js
		Child.prototype = new Parent();
		```
		is that former can create an object that doesn't inherit from anything, ex: `Object.create(null)`, on the other hand, if you set `Child.prototype = null;` the newly created object will inherit from `Object.prototype`. [[stackoverflow]](https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction)