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

### "this"

The value of the ``this`` keyword is based completely on how its function (or method) is called. this could be any of the following:

1. **A new object**
```javascript
const mySundae = new Sundae('Chocolate', ['Sprinkles', 'Hot Fudge']);
```
The value of ``this`` inside the Sundae constructor function is a new object

2. **A specified object**
```JavaScript
const result = obj1.printName.call(obj2);
```
The value of ``this`` inside ``printName()`` will refer to ``obj2`` since the first parameter of ``call()`` is to explicitly set what ``this`` refers to.

3. **A context object**
```javascript
data.teleport();
```
The value of ``this`` inside ``teleport()`` will refer to data.

4. **The global object or undefined**
```javascript
teleport();
```
The value of ``this`` inside ``teleport()`` is either the global object or, if in strict mode, it's ``undefined``.

###### Arrow functions and "this"

With regular functions, the value of ``this`` is set based on how the function is called. With arrow functions, the value of ``this`` is based on the function's surrounding context. In other words, the value of ``this`` inside an arrow function is the same as the value of this outside the function.

### Default function parameters

To create a default parameter, you add an equal sign (``=``) and then whatever you want the parameter to default to if an argument is not provided.

```javascript
function greet(name = 'Student', greeting = 'Welcome') {
  return `${greeting} ${name}!`;
}

greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!
```

###### Defaults and destructuring arrays

```JavaScript
function createGrid([width = 5, height = 5]) {
  return `Generates a ${width} x ${height} grid`;
}

createGrid([]); // Generates a 5 x 5 grid
createGrid([2]); // Generates a 2 x 5 grid
createGrid([2, 3]); // Generates a 2 x 3 grid
createGrid([undefined, 3]); // Generates a 5 x 3 grid

createGrid(); // throws an error

function createGrid2([width = 5, height = 5] = []) {
  return `Generating a grid of ${width} by ${height}`;
}

createGrid2(); // Generates a 5 x 5 grid
```

###### Defaults and destructuring objects

```javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']}) {
  const scoopText = scoops === 1 ? 'scoop' : 'scoops';
  return `Your sundae has ${scoops} ${scoopText} with ${toppings.join(' and ')} toppings.`;
}

createSundae({}); // Your sundae has 1 scoop with Hot Fudge toppings.
createSundae({scoops: 2}); // Your sundae has 2 scoops with Hot Fudge toppings.
createSundae({scoops: 2, toppings: ['Sprinkles']}); // Your sundae has 2 scoops with Sprinkles toppings.
createSundae({toppings: ['Cookie Dough']}); // Your sundae has 1 scoop with Cookie Dough toppings.

createSundae(); // throws an error

function createSundae2({scoops = 1, toppings = ['Hot Fudge']} = {}) {
  const scoopText = scoops === 1 ? 'scoop' : 'scoops';
  return `Your sundae has ${scoops} ${scoopText} with ${toppings.join(' and ')} toppings.`;
}

createSundae2(); // Your sundae has 1 scoop with Hot Fudge toppings.
```

### JavaScript Classes

ES6 classes are just a mirage and hide the fact that prototypal inheritance is actually going on under the hood.

**Class is just a function**. There isn't anything special about class. There isn't even a new type added to JavaScript. Class is a mirage over prototypal inheritance.

###### ES5 "Class" Recap

```JavaScript
function Plane(numEngines) {
  this.numEngines = numEngines;
  this.enginesActive = false;
}

// methods "inherited" by all instances
Plane.prototype.startEngines = function () {
  console.log('starting engines...');
  this.enginesActive = true;
};

const richardsPlane = new Plane(1);
richardsPlane.startEngines();

const jamesPlane = new Plane(4);
jamesPlane.startEngines();
```

###### ES6 Classes

```javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}

typeof Plane; // function

const richardsPlane = new Plane(1);
richardsPlane.startEngines();

const jamesPlane = new Plane(4);
jamesPlane.startEngines();
```

There aren't any commas between the method definitions in the Class. Commas are not used to separate properties or methods in a Class. If you add them, you'll get a SyntaxError of unexpected token.

When creating a new instance of a JavaScript class, the new keyword must be used.

### Static methods

```javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  static badWeather(planes) {
    for (plane of planes) {
      plane.enginesActive = false;
    }
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}

Plane.badWeather([plane1, plane2, plane3]); // badWeather() is a method that's accessed directly on the Plane class
```

### Benefits of classes

1. Less setup
2. Clearly defined constructor function
3. Everything's contained

### Subclasses with ES6

```javascript
class Tree {
  constructor(size = '10', leaves = {spring: 'green', summer: 'green', fall: 'orange', winter: null}) {
    this.size = size;
    this.leaves = leaves;
    this.leafColor = null;
  }

  changeSeason(season) {
    this.leafColor = this.leaves[season];
    if (season === 'spring') {
      this.size += 1;
    }
  }
}

class Maple extends Tree {
  constructor(syrupQty = 15, size, leaves) {
    super(size, leaves);
    this.syrupQty = syrupQty;
  }

  changeSeason(season) {
      super.changeSeason(season);
      if (season === 'spring') {
        this.syrupQty += 1;
      }
    }

    gatherSyrup() {
      this.syrupQty -= 3;
    }
  }

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
```

``Super`` must be called before ``this``. In a subclass constructor function, before this can be used, a call to the super class must be made.
