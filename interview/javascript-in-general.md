## JavaScript

* **Explain event delegation**

  Event delegation is a technique involving adding event listeners to a parent element instead of adding them to the descendant elements. The listener will fire whenever the event is triggered on the descendant elements due to event bubbling up the DOM. The benefits of this technique are:
  
  * Memory footprint goes down because only one single handler is needed on the parent element, rather than having to attach event handlers on each descendant.
  * There is no need to unbind the handler from elements that are removed and to bind the event for new elements.
  
  **References**
  * https://davidwalsh.name/event-delegate
  * https://stackoverflow.com/questions/1687296/what-is-dom-event-delegation

* **Explain how this works in JavaScript**

  There’s no simple explanation for this; it is one of the most confusing concepts in JavaScript. A hand-wavey explanation is that the value of this depends on how the function is called. I have read many explanations on thisonline, and I found Arnav Aggrawal's explanation to be the clearest. The following rules are applied:
  
 1. If the new keyword is used when calling the function, this inside the function is a brand new object.
 2.If apply, call, or bind are used to call/create a function, this inside the function is the object that is passed in as the argument.
 3. If a function is called as a method, such as obj.method() — this is the object that the function is a property of.
 4. If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In a browser, it is the window object. If in strict mode ('use strict'), this will be undefined instead of the global object.
 5. If multiple of the above rules apply, the rule that is higher wins and will set the this value.
 6. If the function is an ES2015 arrow function, it ignores all the rules above and receives the this value of its surrounding scope at the time it is created.
 
 For an in-depth explanation, do check out his article on Medium.
 
 **References**
 * https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3
 * https://stackoverflow.com/a/3127440/1751946
 
* **Explain how prototypal inheritance works**

  This is an extremely common JavaScript interview question. All JavaScript objects have a prototype property, that is a reference to another object. When a property is accessed on an object and if the property is not found on that object, the JavaScript engine looks at the object's prototype. and the prototype's prototype and so on, until it finds the property defined on one of the prototypes or until it reaches the end of the prototype chain. This behaviour simulates classical inheritance, but it is really more of delegation than inheritance.

  **References**
  * https://www.quora.com/What-is-prototypal-inheritance/answer/Kyle-Simpson
  * https://davidwalsh.name/javascript-objects

* **What do you think of AMD vs CommonJS?**

  Both are ways to implement a module system, which was not natively present in JavaScript until ES2015 came along. CommonJS is synchronous while AMD (Asynchronous Module Definition) is obviously asynchronous. CommonJS is designed with server-side development in mind while AMD, with its support for asynchronous loading of modules, is more intended for browsers.
  
  I find AMD syntax to be quite verbose and CommonJS is closer to the style you would write import statements in other languages. Most of the time, I find AMD unnecessary, because if you served all your JavaScript into one concatenated bundle file, you wouldn’t benefit from the async loading properties. Also, CommonJS syntax is closer to Node style of writing modules and there is less context-switching overhead when switching between client side and server side JavaScript development.
  
  I’m glad that with ES2015 modules, that has support for both synchronous and asynchronous loading, we can finally just stick to one approach. Although it hasn’t been fully rolled out in browsers and in Node, we can always use transpilers to convert our code.
  
  **References**
  * https://auth0.com/blog/javascript-module-systems-showdown/
  * https://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs
  
* **Explain why the following doesn’t work as an IIFE: function `foo(){ }()`;. What needs to be changed to properly make it an IIFE?**

  IFE stands for Immediately Invoked Function Expressions. The JavaScript parser reads function `foo(){ }();` as function `foo(){ }` and `();`, where the former is a function declaration and the latter (a pair of brackets) is an attempt at calling a function but there is no name specified, hence it throws `Uncaught SyntaxError: Unexpected token )`.
Here are two ways to fix it that involves adding more brackets: `(function foo(){ })()` and `(function foo(){ }())`. These functions are not exposed in the global scope and you can even omit its name if you do not need to reference itself within the body.

  **References**
  * http://lucybain.com/blog/2014/immediately-invoked-function-expression/
  
* **What’s the difference between a variable that is: null, undefined or undeclared? How would you go about checking for any of these states?**

  Undeclared variables are created when you assign to a value to an identifier that is not previously created using `var`, `let` or `const`. Undeclared variables will be defined globally, outside of the current scope. In strict mode, a `ReferenceError` will be thrown when you try to assign to an undeclared variable. Undeclared variables are bad just like how global variables are bad. Avoid them at all cost! To check for them, wrap its usage in a try/catch block.
  
  ```
  function foo() {
  x = 1;   // Throws a ReferenceError in strict mode
  }
  foo()
  console.log(x) // 1
  ```
  A variable that is `undefined` is a variable that has been declared, but not assigned a value. It is of type `undefined`. If a function does not return any value as the result of executing it is assigned to a variable, the variable also has the value of `undefined`. To check for it, compare using the strict equality `(===)` operator or `typeof` which will give the `'undefined'` string. Note that you should not be using the abstract equality operator to check, as it will also return `true` if the value is `null`.
