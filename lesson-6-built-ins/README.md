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

The ``.next()`` method is a zero arguments function that returns an object with two properties:
* ``value`` : the data representing the next value in the sequence of values within the object
* ``done`` : a boolean representing if the iterator is done going through the sequence of values:
   * If done is true, then the iterator has reached the end of its sequence of values.
   * If done is false, then the iterator is able to produce another value in its sequence of values.

```javascript
const james = {
    name: 'James',
    height: `5'10"`,
    weight: 185,
    [Symbol.iterator]: function () {
        let keys = Object.keys(this);
        let index = 0;
        return {
           next: () => {
               return {
                    key: keys[index],
                    value: this[keys[index]],
                    done: ++index >= keys.length
               }
           }
        };           
    }
};

const iterator = james[Symbol.iterator]();

console.log(iterator.next().value); // 'James'
console.log(iterator.next().value); // `5'10`
console.log(iterator.next().value); // 185
```

## Sets

Set is a collection of distinct items. The biggest differences between a set and an array are:

* Sets are not indexed-based - you do not refer to items in a set based on their position in the set.
* Items in a Set can’t be accessed individually.

```javascript
const games = new Set();
console.log(games);
// Set {}
```

If you create a Set from a list of values, ES6 will automatically remove the duplicate entry.

```javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);
console.log(games);
// Set {'Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart'}
```

### Modifying Sets

```javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);

games.add('Banjo-Tooie');
games.add('Age of Empires');

games.delete('Super Mario Bros.');

games.clear()
```

If you attempt to ``.add()`` a duplicate item to a Set, you won’t receive an error, but the item will not be added to the Set. Also, if you try to ``.delete()`` an item that is not in a Set, you won’t receive an error, and the Set will remain unchanged.

``.add()`` returns the ``Set`` if an item is successfully added. On the other hand, ``.delete()`` returns a Boolean (true or false) depending on successful deletion.

### Working With Sets

Use the ``.size`` property instead of ``.length`` property to get the size of the Set.

```javascript
console.log(games.size);
```

Use the ``.has()`` method to check if an item exists in a Set. If the item is in the Set, then ``.has()`` will return true. If the item doesn’t exist in the Set, then ``.has()`` will return false.

```javascript
console.log(games.has('September'));
```

Use the ``.values()`` method to return the values in a Set. The return value of the ``.values()`` method is a ``SetIterator`` object.

```javascript
console.log(games.values());
```

The ``.keys()`` method will behave the exact same way as the ``.values()`` method by returning the values of a Set within a new Iterator Object. The ``.keys()`` method is an alias for the ``.values()`` method for similarity with maps.

### Using the SetIterator

```JavaScript
const iterator = months.values();
iterator.next();
// Object {value: 'January', done: false}
```

### Using a for...of Loop with Sets

```javascript
const colors = new Set(['red', 'orange', 'yellow', 'green', 'blue', 'violet', 'brown', 'black']);
for (const color of colors) {
  console.log(color);
}
```

## WeakSet

WeakSet is just like a normal Set with a few key differences:

* a WeakSet can only contain objects
* a WeakSet is not iterable which means it can’t be looped over
* a WeakSet does not have a ``.clear()`` method

```javascript
const student1 = { name: 'James', age: 26, gender: 'male' };
const student2 = { name: 'Julia', age: 27, gender: 'female' };
const student3 = { name: 'Richard', age: 31, gender: 'male' };

const roster = new WeakSet([student1, student2, student3]);
```

If you try to add something other than an object, you’ll get an error.

```javascript
roster.add('Amanda');
// Uncaught TypeError: Invalid value used in weak set(…)
```

### Garbage Collection

WeakSets take advantage of garbage collection by exclusively working with objects. If you set an object to null, then you’re essentially deleting the object. And when JavaScript’s garbage collector runs, the memory that object previously occupied will be freed up to be used later in your program.

## Maps

If Sets are similar to Arrays, then Maps are similar to Objects because Maps store key-value pairs similar to how objects contain named properties with values.

Essentially, a Map is an object that lets you store key-value pairs where both the keys and the values can be objects, primitive values, or a combination of the two.

```javascript
const employees = new Map();
console.log(employees);
// Map {}
```

### Modifying Maps

```javascript
const employees = new Map();

employees.set('james.parkes@udacity.com', {
    firstName: 'James',
    lastName: 'Parkes',
    role: 'Content Developer'
});

employees.delete('julia@udacity.com');

employees.clear()
```

If you ``.set()`` a key-value pair to a Map that already uses the same key, you won’t receive an error, but the key-value pair will overwrite what currently exists in the Map. Also, if you try to ``.delete()`` a key-value that is not in a Map, you won’t receive an error, and the Map will remain unchanged.

The ``.delete()`` method returns true if a key-value pair is successfully deleted from the Map object, and false if unsuccessful. The return value of .set() is the Map object itself if successful.

### Working with Maps

Use the ``.has()`` method to check if a key-value pair exists in your Map by passing it a key.

```javascript
onst members = new Map();

members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

console.log(members.has('Xavier'));
console.log(members.has('Marcus'));

// false
// true
```

You can  retrieve values from a Map, by passing a key to the ``.get()`` method.

```javascript
console.log(members.get('Evelyn'));
// 75.68
```

### Using the MapIterator

```javascript
let iteratorObjForKeys = members.keys();
iteratorObjForKeys.next();
//  Object {value: 'Evelyn', done: false}
```

Use the ``.values()`` method to access the Map’s values, and then repeat the same process.

```javascript
let iteratorObjForValues = members.values();
iteratorObjForValues.next();
// Object {value: 75.68, done: false}
```

###  Using a for...of Loop

```javascript
for (const member of members) {
  console.log(member);
}
```

### Using a forEach Loop

```javascript
members.forEach((key, value) => console.log(key, value));
```

## WeakMap

WeakMap is just like a normal Map with a few key differences:

* a WeakMap can only contain objects as keys,
* a WeakMap is not iterable which means it can’t be looped and
* a WeakMap does not have a ``.clear()`` method.

```javascript
const book1 = { title: 'Pride and Prejudice', author: 'Jane Austen' };
const book2 = { title: 'The Catcher in the Rye', author: 'J.D. Salinger' };
const book3 = { title: 'Gulliver’s Travels', author: 'Jonathan Swift' };

const library = new WeakMap();
library.set(book1, true);
library.set(book2, false);
library.set(book3, true);

console.log(library);
```

If you try to add something other than an object as a key, you’ll get an error.

```javascript
library.set('The Grapes of Wrath', false);
// Uncaught TypeError: Invalid value used as weak map key(…)
```

## Promises

A JavaScript Promise is created with the new Promise constructor function - new Promise(). A promise will let you start some work that will be done asynchronously and let you get back to your regular work. When you create the promise, you must give it the code that will be run asynchronously. You provide this code as the argument of the constructor function.

```javascript
new Promise(function () {
    window.setTimeout(function createSundae(flavor = 'chocolate') {
        const sundae = {};
        // request ice cream
        // get cone
        // warm up ice cream scoop
        // scoop generous portion into cone!
    }, Math.random() * 2000);
});
```

### Indicated a Successful Request or a Failed Request

Once that's all done, JavaScript notifies us that it's finished and ready for us to pick back up by passing two functions into our initial function. Typically we call these ``resolve`` and ``reject``.


```javascript
new Promise(function (resolve, reject) {
    window.setTimeout(function createSundae(flavor = 'chocolate') {
        const sundae = {};
        // request ice cream
        // get cone
        // warm up ice cream scoop
        // scoop generous portion into cone!
        resolve(sundae);
    }, Math.random() * 2000);
});
```

### Promises Return Immediately

```javascript
const myPromiseObj = new Promise(function (resolve, reject) {
    // sundae creation code
});
```

That object has a ``.then()`` method on it that we can use to have it notify us if the request we made in the promise was either successful or failed. The ``.then()`` method takes two functions:

* the function to run if the request completed successfully
* the function to run if the request failed to complete

```JavaScript
mySundae.then(function(sundae) {
    console.log(`Time to eat my delicious ${sundae}`);
}, function(msg) {
    console.log(msg);
    self.goCry(); // not a real method
});
```

The first function that's passed to ``.then(``) will be called and passed the data that the Promise's resolve function used. The second function will be called and passed the data that the Promise's reject function was called with.

## Proxies

A proxy object sits between a real object and the calling code. The calling code interacts with the proxy instead of the real object.

To create a proxy object, we use the Proxy constructor - ``new Proxy();``. The proxy constructor takes two items:

* the object that it will be the proxy for
* an object containing the list of methods it will handle for the proxied object

The second object is called the handler.

```javascript
var richard = {status: 'looking for work'};
var agent = new Proxy(richard, {});

agent.status; // returns 'looking for work'
```

### Get Trap

```javascript
const richard = {status: 'looking for work'};
const handler = {
    get(target, propName) {
        console.log(target); // the `richard` object, not `handler` and not `agent`
        console.log(propName); // the name of the property the proxy (`agent` in this case) is checking
    }
};
const agent = new Proxy(richard, handler);
agent.status; // logs out the richard object (not the agent object!) and the name of the property being accessed (`status`)
```

If a trap is used, you need to make sure you provide all the functionality for that specific trap.

### Accessing the Target object from inside the proxy

```JavaScript
const handler = {
    get(target, propName) {
        console.log(target);
        console.log(propName);
        return target[propName];
    }
};
```

### Set Trap

```JavaScript
const richard = {status: 'looking for work'};
const handler = {
    set(target, propName, value) {
        if (propName === 'payRate') { // if the pay is being set, take 15% as commission
            value = value * 0.85;
        }
        target[propName] = value;
    }
};
const agent = new Proxy(richard, handler);
agent.payRate = 1000; // set the actor's pay to $1,000
agent.payRate; // $850 the actor's actual pay
```

### Other traps

1. the get trap - lets the proxy handle calls to property access
1. the set trap - lets the proxy handle setting the property to a new value
1. the apply trap - lets the proxy handle being invoked (the object being proxied is a function)
1. the has trap - lets the proxy handle the using in operator
1. the deleteProperty trap - lets the proxy handle if a property is deleted
1. the ownKeys trap - lets the proxy handle when all keys are requested
1. the construct trap - lets the proxy handle when the proxy is used with the new keyword as a constructor
1. the defineProperty trap - lets the proxy handle when defineProperty is used to create a new property on the object
1. the getOwnPropertyDescriptor trap - lets the proxy handle getting the property's descriptors
1. the preventExtenions trap - lets the proxy handle calls to Object.preventExtensions() on the proxy object
1. the isExtensible trap - lets the proxy handle calls to Object.isExtensible on the proxy object
1. the getPrototypeOf trap - lets the proxy handle calls to Object.getPrototypeOf on the proxy object
1. the setPrototypeOf trap - lets the proxy handle calls to Object.setPrototypeOf on the proxy object

## Generators

If we do want to be able to pause a function mid-execution, then we'll need a new type of function available to us in ES6 - generator functions!

```javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        console.log( name );
    }

    console.log('the function has ended');
}
```

Asterisk indicates that this function is actually a generator. The asterisk of the generator can actually be placed anywhere between the function keyword and the function's name. The community has coalesced into having the asterisk appear right next to the function keyword (i.e. function* name() { … }). But there others that recommend having the asterisk touch the function's name instead. So it's important to realize that the asterisk indicates that it is a generator but that the placement of the asterisk is not important.

When a generator is invoked, it doesn't actually run any of the code inside the function. Instead, it creates and returns an iterator. This iterator can then be used to execute the actual generator's inner code.

```javascript
getEmployee();

// this is the response I get in Chrome:
getEmployee {[[GeneratorStatus]]: "suspended", [[GeneratorReceiver]]: Window}

const generatorIterator = getEmployee();
generatorIterator.next();
```

### Yield

It can only be used inside generator functions. ``yield`` is what causes the generator to pause.

```javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        console.log(name);
        yield;
    }

    console.log('the function has ended');
}
```

### Yielding Data to the "Outside" World

```javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        yield name;
    }

    console.log('the function has ended');
}

const generatorIterator = getEmployee();
let result = generatorIterator.next();
result.value // is "Amanda"

generatorIterator.next().value // is "Diego"
generatorIterator.next().value // is "Farrin"
```

### Sending Data into/out of a Generator

```javascript
function* displayResponse() {
    const response = yield;
    console.log(`Your response is "${response}"!`);
}

const iterator = displayResponse();

iterator.next(); // starts running the generator function
iterator.next('Hello Udacity Student'); // send data into the generator
// the line above logs to the console: Your response is "Hello Udacity Student"!
```

The ``yield`` keyword is used to pause a generator and used to send data outside of the generator, and then the ``.next()`` method is used to pass data into the generator.

```JavaScript
function* getEmployee() {
    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];
    const facts = [];

    for (const name of names) {
        // yield *out* each name AND store the returned data into the facts array
        facts.push(yield name);
    }

    return facts;
}

const generatorIterator = getEmployee();

// get the first name out of the generator
let name = generatorIterator.next().value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is cool!`).value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is awesome!`).value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is stupendous!`).value;

// you get the idea
name = generatorIterator.next(`${name} is rad!`).value;
name = generatorIterator.next(`${name} is impressive!`).value;
name = generatorIterator.next(`${name} is stunning!`).value;
name = generatorIterator.next(`${name} is awe-inspiring!`).value;

// pass the last data in, generator ends and returns the array
const positions = generatorIterator.next(`${name} is magnificent!`).value;

// displays each name with description on its own line
positions.join('\n');
```
