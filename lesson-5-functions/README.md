# Functions

### Arrow functions

* Arrow functions are very similar to regular functions in behavior, but are quite different syntactically  ( ```=>``` ).
* Regular functions can be either function *declarations* or function *expressions*, however arrow functions are always *expressions* - there's no such thing as an arrow function declaration.

###### Convert a function to an arrow function

1. remove the function keyword
1. remove the parentheses
1. remove the opening and closing curly braces
1. remove the return keyword
1. remove the semicolon
1. add an arrow ( => ) between the parameter list and the function body

###### Example of regular and arrow functions

Regular function:

```JavaScript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(function(name) {
  return name.toUpperCase();
});
```

Arrow function:

```JavaScript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```

###### Parentheses and arrow function parameteres

```JavaScript
const greet = name => `Hello ${name}!`;
greet('Asser');

const sayHi = () => console.log('Hello Udacity Student!');
sayHi();

const orderIceCream = (flavor, cone) => console.log(`Here's your ${flavor} ice cream in a ${cone} cone.`);
orderIceCream('chocolate', 'waffle');
```

###### Concise body syntax

* has no curly braces surrounding the function body,
* automatically returns the expression.

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```

###### Block body syntax

* used when you need more than just a single line of code in your arrow function's body,
* it uses curly braces to wrap the function body,
* it returns statement needs to be used to actually return something from the function.

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map( name => {
  name = name.toUpperCase();
  return `${name} has ${name.length} characters in their name`;
});
```

###### Advantages of using arrow Functions

* The syntax is a lot shorter.
* It's easier to write and read short, single-line functions.
* They automatically return when using the concise body syntax.
