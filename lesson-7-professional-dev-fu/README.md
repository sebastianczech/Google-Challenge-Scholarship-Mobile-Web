# Professional Developer-fu

* [ECMAScript® 2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/index.html)
* [Mozilla Developer Network](https://developer.mozilla.org)
* [ECMAScript Compatibility Table](http://kangax.github.io/compat-table/es6/)
* [What is a Polyfill?](https://remysharp.com/2010/10/08/what-is-a-polyfill)
* [HTML5 Cross Browser Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)
* [Babel](https://babeljs.io/)
* [ES2015 preset](http://babeljs.io/docs/plugins/preset-es2015/)

## Polyfill

A polyfill, or polyfiller, is a piece of code (or plugin) that provides the technology that you, the developer, expect the browser to provide natively.

### Example

```javascript
if (!String.prototype.startsWith) {
  String.prototype.startsWith = function (searchString, position) {
    position = position || 0;
    return this.substr(position, searchString.length) === searchString;
  };
}
```

## Babel

Babel's original name was slightly more descriptive - 6to5. This was because, originally, Babel converted ES6 code to ES5 code. Now, Babel does a lot more. It'll convert ES6 to ES5, JSX to JavaScript, and Flow to JavaScript.
