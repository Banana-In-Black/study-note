# Map, Set, WeakSet and WeakMap

## Set

The Set object lets you store unique values of any type, whether primitive values or object references.

- Operation on data through `add(value)`, `delete(value)`, `has(value)` and `clear()`.
- Get the number of values in set through `size` property.
- Only contain values. (No keys)
- Enumerable.
  - `values()`, `keys()` or `entries()` will still work. (_key_ and _value_ are the same)
  - `forEach(callback[, thisArg])` while callback accept `(value, key, set)`. (_key_ and _value_ are the same)

### WeakSet

- Don't have `clear()` and `size` property.
- It's a collections of objects only. They cannot contain arbitrary values of any type, as Sets can.
- _Weak_ meaning references to objects in a `WeakSet` are held weakly. If __no other references__ to an object stored in the `WeakSet` exist, those objects can be garbage collected. (The reference `WeakSet` held to objects doesn't count, because it's _weak_!)
- And because it's _weak_, `WeakSet`s are not enumerable. (There's no stable list of current objects)

## Map

The `Map` object holds key-value pairs and remembers the original insertion order of the keys. Any value (both objects and primitive values) may be used as either a key or a value. [Sometimes `Map` is preferable than object in certain cases.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map#Objects_vs._Maps)

- Operation on data through `set(key, value)`, `get(key)`, `delete(key)`, `has(key)` and `clear()`.
- Get the number of key-value pairs in map through `size` property.
- Enumerable. (`values()`, `forEach(function(value, key, map)[, thisArg])`, etc.)

### WeakMap

- Don't have `clear()` and `size` property.
- Keys of a `WeakMap` are of the type `Object` only. But values can be arbitrary values.
- `WeakMap` holds _"weak"_ references to key objects, which means that they do not prevent garbage collection in case there would be no other reference to the key object. This also avoids preventing garbage collection of values in the map.
- And because it's _weak_, `WeakMap`s are not enumerable. (No stable list of key)
