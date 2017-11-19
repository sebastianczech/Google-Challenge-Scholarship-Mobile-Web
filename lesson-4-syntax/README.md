# Syntax

> Harmony === ES6 === ES2015

### Let and Const

* Variables declared with `let` can be reassigned and can’t be redeclared in the same scope.
* Variables declared with `const` must be assigned an initial value and can’t be redeclared in the same scope and can’t be reassigned.
* There is no reason to use `var` anymore.

### Template Literals

* Template literals are essentially string literals that include embedded expressions.
* Denoted with backticks (\`\`) template literals can contain placeholders which are represented using ```${expression}``` e.g.:

```javascript
let message = `${obj.attr} concated with this text.`;
```

* Easy to write multi-line strings - template literals preserve newlines as part of the string.
* Embedded expressions inside template literals can perform operations, call functions and use loops.

### Destructuring

* Destructuring allows you to specify the elements you want to extract from an array or object on the left side of an assignment e.g.:

###### Destructuring from array

```javascript
const table = [1, 2];
const [a, b] = table;
```

###### Destructuring from object

```javascript
const obj = {
  a: 'test',
  b: 27
};
const {a, b} = obj;
```

### Object literal shorthand

* If object properties have the same name as the variables being assigned to them, then you can drop the duplicate variable names. Instead of using:

```javascript
let a = 'test';
let b = 27;

const obj = {
  a: a,
  b: b
};
```

use following:

```javascript
let a = 'test';
let b = 27;

const obj = {a, b};
```
* There is also shorthand way for method names. Instead of using:

```javascript
let a = 'test';
let b = 27;

const obj = {
  a,
  b,
  calculation: function() {
    // ...
  }
};
```

use following:

```javascript
let a = 'test';
let b = 27;

const obj = {
  a,
  b,
  calculation() {
    // ...
  }
};
```

### For loops

###### For loop

```javascript
const table = [0, 1, 2, 3, 4, 5, 6, 7];
for (let i = 0; i < table.length; i++) {
  console.log(table[i]);
}
```

###### For...in loop

```javascript
const table = [0, 1, 2, 3, 4, 5, 6, 7];

for (const index in table) {
  console.log(table[index]);
}
```

> The `forEach` loop is another type of for loop in JavaScript. However, `forEach()` is actually an array method, so it can only be used exclusively with arrays. There is also no way to stop or break a `forEach` loop. If you need that type of behavior in your loop, you’ll have to use a basic for loop.

###### For...of loop

```javascript
const table = [0, 1, 2, 3, 4, 5, 6, 7];

for (const element of table) {
  console.log(element);
}
```

>  It’s good practice to use plural names for objects that are collections of values. That way, when you loop over the collection, you can use the singular version of the name when referencing individual values in the collection.

### Spread... operator

* The spread operator, written with three consecutive dots ( ```...``` ) gives you the ability to expand, or spread, iterable objects into multiple elements e.g.:

```javascript
const text = ["Simple", "text"];
console.log(...text);
```

* Spread operator can be useful is when combining arrays e.g.:

```javascript
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];

const produce = [...fruits, ...vegetables];
```

### ...Rest Parameter

* The rest parameter, also written with three consecutive dots ( ... ), allows you to represent an indefinite number of elements as an array.
* This can be helpful when assigning the values of an array to variables e.g.:

```javascript
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
```

* Rest parameter is like the opposite of the spread operator; if the spread operator is like unboxing all of the contents of a package, then the rest parameter is like taking all the contents and putting them back into the package.
* Another use case for the rest parameter is when you’re working with variadic functions. Variadic functions are functions that take an indefinite number of arguments e.g.:

```javascript
sum(1, 2);
sum(10, 36, 7, 84, 90, 110);
sum(-23, 3000, 575000);

function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
