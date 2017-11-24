# Built-ins

## Symbols

A symbol is a unique and immutable data type that is often used to identify object properties. To create a symbol, you write ``Symbol()`` with an optional string as its description.

```JavaScript
const sym1 = Symbol('apple');
console.log(sym1);
```

Description is not used as part of the symbol itself - each time a new symbol is created, regardless of the description.

```javascript
const bowl = {
  'apple': { color: 'red', weight: 136.078 },
  'banana': { color: 'yellow', weight: 183.151 },
  'orange': { color: 'orange', weight: 170.097 },
  'banana': { color: 'yellow', weight: 176.845 }
};
// Object {apple: Object, banana: Object, orange: Object}
```

```JavaScript
const bowl = {
  [Symbol('apple')]: { color: 'red', weight: 136.078 },
  [Symbol('banana')]: { color: 'yellow', weight: 183.15 },
  [Symbol('orange')]: { color: 'orange', weight: 170.097 },
  [Symbol('banana')]: { color: 'yellow', weight: 176.845 }
};
// Object {Symbol(apple): Object, Symbol(banana): Object, Symbol(orange): Object, Symbol(banana): Object}
```

## The Iterable Protocol

The iterable protocol is used for defining and customizing the iteration behavior of objects. For some objects (e.g. strings and arrays), they already come built-in with this behavior.

```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
for (const digit of digits) {
  console.log(digit);
}
// 0
// 1
// ..
// 9
```

Any object that is iterable can use the new ``for...of`` loop.

In order for an object to be iterable, it must implement the iterable interface. In order for an object to be iterable it must contain a default iterator method. This method will define how the object should be iterated.

The iterator method, which is available via the constant ``[Symbol.iterator]``, is a zero arguments function that returns an iterator object. An iterator object is an object that conforms to the iterator protocol.

## The Iterator Protocol

The iterator protocol is used to define a standard way that an object produces a sequence of values. What that really means is you now have a process for defining how an object will iterate. This is done through implementing the ``.next()`` method.

The ``.next()`` method is a zero arguments function that returns an object with two properties:
* ``value`` : the data representing the next value in the sequence of values within the object
* ``done`` : a boolean representing if the iterator is done going through the sequence of values:
   * If done is true, then the iterator has reached the end of its sequence of values.
   * If done is false, then the iterator is able to produce another value in its sequence of values.

```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
const arrayIterator = digits[Symbol.iterator]();

console.log(arrayIterator.next());
console.log(arrayIterator.next());
console.log(arrayIterator.next());

// Object {value: 0, done: false}
// Object {value: 1, done: false}
// Object {value: 2, done: false}
```   
