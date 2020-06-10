# Prototype Chain and Classes

## Prototype chain [[doc1]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) [[doc2]](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype.  

When trying to access a property of an object, the property will not only be sought on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.

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
- ES6 Class keywords: `extends`, `super` [[anchor]](#extend)
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

	- Modify `prototype.constructor` won't have an effect while you initiate an object (by using `new`). It's only useful while you need a constructor reference from instance. [[stackoverflow1]](https://stackoverflow.com/questions/8453887/why-is-it-necessary-to-set-the-prototype-constructor) [[stackoverflow2]](https://stackoverflow.com/questions/9267157/why-is-it-impossible-to-change-constructor-function-from-prototype)
	- The difference between
		```js
		Child.prototype = Object.create(Parent.prototype);
		```
		and
		```js
		Child.prototype = new Parent();
		```
		is that former can create an object that doesn't inherit from anything, ex: `Object.create(null)`, on the other hand, if you set `Child.prototype = null;` the newly created object will inherit from `Object.prototype`. [[stackoverflow]](https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction) 

## Classes [[doc]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- `class` body is executed in strict mode.
- Class instance(private & public) / static fields declaration are still proposals.

### Define classes
- A class can't be redefined. Attempting to do so produces a `SyntaxError`.
- Class declaration: Unlike function declaration, it's **hoisted but not initialized**. [[stackoverflow]](https://stackoverflow.com/a/31222689)
	```js
	var p = new Rectangle(); // ReferenceError: Rectangle is not defined

	class Rectangle { }
	```
- Class expression
	```js
	var Unnamed = class { };
	var NamedClass = class Named { };
	console.log(Unnamed.name); // Unnamed
	console.log(NamedClass.name); // Named
	```

### Static method
```js
class Point {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}

	static distance(a, b) {
		const dx = a.x - b.x;
		const dy = a.y - b.y;

		return Math.hypot(dx, dy);
	}
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
p1.distance; //undefined
p2.distance; //undefined

console.log(Point.distance(p1, p2)); // 7.0710678118654755
```

Before ES6:
```js
function Point(x, y) {
	this.x = x;
	this.y = y;
}

Point.distance = function(a, b) {
	var dx = a.x - b.x;
	var dy = a.y - b.y;

	return Math.hypot(dx, dy);
};
```

### Sub classing with `extend` <a id="extend"></a>
- Classes can only extend one superclass.
- Classes cannot extend regular (non-constructible) objects. If you want to inherit from a regular object, you can instead use `Object.setPrototypeOf()`.
- If there is a constructor present in the subclass, it needs to first call `super()` before using `this`.

```js
class Animal { 
	constructor(name) {
		this.name = name;
	}
	
	speak() {
		console.log(`${this.name} makes a noise.`);
	}
}

class Dog extends Animal {
	constructor(name) {
		super(name); // call the super class constructor and pass in the name parameter
	}

	speak() {
		console.log(`${this.name} barks.`);
	}
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```

### Mix-ins
A function with a superclass as input and a subclass extending that superclass as output can be used to implement mix-ins in ECMAScript:

```js
let calculatorMixin = Base => class extends Base {
  	calc() { }
};

let randomizerMixin = Base => class extends Base {
  	randomize() { }
};

class Foo { }
class Bar extends calculatorMixin(randomizerMixin(Foo)) { }
```