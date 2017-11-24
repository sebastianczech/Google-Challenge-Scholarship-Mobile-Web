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
