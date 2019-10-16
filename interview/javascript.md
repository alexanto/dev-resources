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
  
  ```
  var foo;
  console.log(foo); // undefined
  console.log(foo === undefined); // true
  console.log(typeof foo === 'undefined'); // true
  console.log(foo == null); // true. Wrong, don't use this to check!
  function bar() {}
  var baz = bar();
  console.log(baz); // undefined
  ```
  
  
  A variable that is null will have been explicitly assigned to the null value. It represents no value and is different from undefined in the sense that it has been explicitly assigned. To check for `null`, simply compare using the strict equality operator. Note that like the above, you should not be using the abstract equality operator `(==)` to check, as it will also return `true` if the value is `undefined`.
  
  ```
  var foo = null;
  console.log(foo === null); // true
  console.log(foo == undefined); // true. Wrong, don't use this to check!
  ```
  
  As a personal habit, I never leave my variables undeclared or unassigned. I will explicitly assign `null` to them after declaring, if I don't intend to use it yet.
  
  **References**
  * https://stackoverflow.com/questions/15985875/effect-of-declared-and-undeclared-variables
  * https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/undefined
  
* **Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?**

  This question is pretty vague. My best guess at its intention is that it is asking about constructors in JavaScript. Technically speaking, `function Person(){}` is just a normal function declaration. The convention is use PascalCase for functions that are intended to be used as constructors.
`var person = Person()` invokes the Person as a function, and not as a constructor. Invoking as such is a common mistake if it the function is intended to be used as a constructor. Typically, the constructor does not return anything, hence invoking the constructor like a normal function will return undefined and that gets assigned to the variable intended as the instance.
`var person = new Person()` creates an instance of the Person object using the new operator, which inherits from `Person.prototype`. An alterative would be to use `Object.create`, such as: `Object.create(Person.prototype)`.

  ```
  function Person(name) {
    this.name = name;
  }
  var person = Person('John');
  console.log(person); // undefined
  console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined
  var person = new Person('John');
  console.log(person); // Person { name: "John" }
  console.log(person.name); // "john"
  ```

   **References**
   * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new
  
* **What’s the difference between .call and .apply?**

  Both `.call` and `.apply` are used to invoke functions and the first parameter will be used as the value of this within the function. However, `.call` takes in a comma-separated arguments as the next arguments while `.apply` takes in an array of arguments as the next argument. An easy way to remember this is C for call and comma-separated and A for apply and array of arguments.


```
function add(a, b) {
  return a + b;
}
console.log(add.call(null, 1, 2)) // 3
console.log(add.apply(null, [1, 2])) // 3
```

* **Explain Function.prototype.bind.**

  Taken word-for-word from MDN:
  The `bind()` method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.
  In my experience, it is most useful for binding the value of this in methods of classes that you want to pass into other functions. This is frequently done in React components.

  **References**
  * https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind
  
* **When would you use `document.write()`?**

  `document.write()` writes a string of text to a document stream opened by `document.open()`. When `document.write()` is executed after the page has loaded, it will call document.open which clears the whole document (`<head>` and `<body>`removed!) and replaces the contents with the given parameter value in string. Hence it is usually considered dangerous and prone to misuse.
There are some answers online that explain `document.write()` is being used in analytics code or when you want to include styles that should only work if JavaScript is enabled. It is even being used in HTML5 boilerplate to load scripts in parallel and preserve execution order! However, I suspect those reasons might be outdated and in the modern day, they can be achieved without using `document.write()`. Please do correct me if I'm wrong about this.
  
  **References**
  * https://www.quirksmode.org/blog/archives/2005/06/three_javascrip_1.html
  * https://github.com/h5bp/html5-boilerplate/wiki/Script-Loading-Techniques#documentwrite-script-tag
  
* **Have you ever used JavaScript templating? If so, what libraries have you used?**

  Yes. Handlebars, Underscore, Lodash, AngularJS and JSX. I disliked templating in AngularJS because it made heavy use of strings in the directives and typos would go uncaught. JSX is my new favourite as it is closer to JavaScript and there is barely and syntax to be learnt. Nowadays, you can even use ES2015 template string literals as a quick way for creating templates without relying on third-party code.
 
```
const template = `<div>My name is: ${name}</div>`;
```

However, do beware of a potential XSS in the above approach as the contents are not escaped for you, unlike in templating libraries.

* **What is the difference between == and ===?**

  `==` is the abstract equality operator while `===` is the strict equality operator. The `==` operator will compare for equality after doing any necessary type conversions. The `===` operator will not do type conversion, so if two values are not the same type `===` will simply return false. When using `==`, funky things can happen, such as:
  
  ```
  1 == '1' // true
  1 == [1] // true
  1 == true // true
  0 == '' // true
  0 == '0' // true
  0 == false // true
  ```

   My advice is never to use the `==` operator, except for convenience when comparing against `null` or `undefined`, where a `== null` will return `true` if a is `null` or `undefined`.
  
  ```
  var a = null;
  console.log(a == null); // true
  console.log(a == undefined); // true
  ```

    **References**
    * https://stackoverflow.com/questions/359494/which-equals-operator-vs-should-be-used-in-javascript-comparisons

* **Make this work:**

  ```
  duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
  function duplicate(arr) {
    return arr.concat(arr);
  }
  ```

* **Why is it called a Ternary expression, what does the word “Ternary” indicate?**

  “Ternary” indicates three, and a ternary expression accepts three operands, the test condition, the “then” expression and the “else” expression. Ternary expressions are not specific to JavaScript and I’m not sure why it is even in this list.

  **References**
  * https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator
  
  
* **What is "use strict";? What are the advantages and disadvantages to using it?**

  ‘use strict’ is a statement used to enable strict mode to entire scripts or individual functions. Strict mode is a way to opt in to a restricted variant of JavaScript.
  
  Advantages:
  * Makes it impossible to accidentally create global variables.
  * Makes assignments which would otherwise silently fail to throw an exception.
  * Makes attempts to delete undeletable properties throw (where before the attempt would simply have no effect).
  * Requires that function parameter names be unique.
  * `this` is undefined in the global context.
  * It catches some common coding bloopers, throwing exceptions.
  * It disables features that are confusing or poorly thought out.
  
  Disadvantages:
  * Many missing features that some developers might be used to.
  * No more access to function.caller and function.arguments.
  * Concatenation of scripts written in different strict modes might cause issues.
  * Overall, I think the benefits outweigh the disadvantages, and I never had to rely on the features that strict mode blocks. I would recommend using strict mode.
  
  **References**
  * http://2ality.com/2011/10/strict-mode-hatred.html
  * http://lucybain.com/blog/2014/js-use-strict/
  
* **Create a for loop that iterates up to 100 while outputting "fizz" at multiples of 3, "buzz" at multiples of 5 and "fizzbuzz" at multiples of 3 and 5.**

  ```
  for (let i = 1; i <= 100; i++) {
    let f = i % 3 == 0, b = i % 5 == 0;
    console.log(f ? (b ? 'FizzBuzz' : 'Fizz') : (b ? 'Buzz' : i));
  }
  ```
  
  I would not advise you to write the above during interviews though. Just stick with the long but clear approach. For more wacky versions of FizzBuzz, check out the reference link below.
  **References**
  * https://gist.github.com/jaysonrowe/1592432
  
* **Explain what a single page app is and how to make one SEO-friendly.**

  Web developers these days refer to the products they build as web apps, rather than websites. While there is no strict difference between the two terms, web apps tend to be highly interactive and dynamic, allowing the user to perform actions and receive a response for their action. Traditionally, the browser receives HTML from the server and renders it. When the user navigates to another URL, a full-page refresh is required and the server sends fresh new HTML for the new page. This is called server-side rendering.
However in modern SPAs, client-side rendering is used instead. The browser loads the initial page from the server, along with the scripts (frameworks, libraries, app code) and stylesheets required for the whole app. When the user navigates to other pages, a page refresh is not triggered. The URL of the page is updated via the HTML5 History API. New data required for the new page, usually in JSON format, is retrieved by the browser via AJAX requests to the server. The SPA then dynamically updates the page with the data via JavaScript, which it has already downloaded in the initial page load. This model is similar to how native mobile apps work.

  The benefits:
  * The app feels more responsive and users do not see the flash between page navigations due to full-page refreshes.
  * Fewer HTTP requests are made to the server, as the same assets do not have to be downloaded again for each page load.
  * Clear separation of the concerns between the client and the server; you can easily build new clients for different platforms (e.g. mobile, chatbots, smart watches) without having to modify the server code. You can also modify the technology stack on the client and server independently, as long as the API contract is not broken.
  
  The downsides:
  * Heavier initial page load due to loading of framework, app code, and assets required for multiple pages.
  * There’s an additional step to be done on your server which is to configure it to route all requests to a single entry point and allow client-side routing to take over from there.
  * SPAs are reliant on JavaScript to render content, but not all search engines execute JavaScript during crawling, and they may see empty content on your page. This inadvertently hurts the Search Engine Optimization (SEO) of your app. However, most of the time, when you are building apps, SEO is not the most important factor, as not all the content needs to be indexable by search engines. To overcome this, you can either server-side render your app or use services such as Prerender to “render your javascript in a browser, save the static HTML, and return that to the crawlers”.
  
  **References**
  * https://github.com/grab/front-end-guide#single-page-apps-spas
  * http://stackoverflow.com/questions/21862054/single-page-app-advantages-and-disadvantages
  * http://blog.isquaredsoftware.com/presentations/2016-10-revolution-of-web-dev/
  * https://medium.freecodecamp.com/heres-why-client-side-rendering-won-46a349fadb52
  
* **What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?**

  Some examples of languages that compile to JavaScript include CoffeeScript, Elm, ClojureScript, PureScript and TypeScript.
  
  Advantages:
  * Fixes some of the longstanding problems in JavaScript and discourages JavaScript anti-patterns.
  * Enables you to write shorter code, by providing some syntactic sugar on top of JavaScript, which I think ES5 lacks, but ES2015 is awesome.
  * Static types are awesome (in the case of TypeScript) for large projects that need to be maintained over time.

  Disadvantages:
  * Require a build/compile process as browsers only run JavaScript and your code will need to be compiled into JavaScript before being served to browsers.
  * Debugging can be a pain if your source maps do not map nicely to your pre-compiled source.
  * Most developers are not familiar with these languages and will need to learn it. There’s a ramp up cost involved for your team if you use it for your projects.
  * Smaller community (depends on the language), which means resources, tutorials, libraries and tooling would be harder to find.
  * IDE/editor support might be lacking.
  * These languages will always be behind the latest JavaScript standard.
  * Practically, ES2015 has vastly improved JavaScript and made it much nicer to write. I don’t really see the need for CoffeeScript these days.
  **References**
  * https://softwareengineering.stackexchange.com/questions/72569/what-are-the-pros-and-cons-of-coffeescript
  
* **What is event loop? What is the difference between call stack and task queue?**

  The event loop is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. If the call stack is empty and there are callback functions in the task queue, a function is dequeued and pushed onto the call stack to be executed.
  If you haven’t already checked out Philip Robert’s talk on the Event Loop, you should. It is one of the most viewed videos on JavaScript.
  
  **References**
  * https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html
  * http://theproactiveprogrammer.com/javascript/the-javascript-event-loop-a-stack-and-a-queue/
  
* **Explain the differences on the usage of foo between function `foo() {}` and `var foo = function() {}`**

  The former is a function declaration while the latter is a function expression. The key difference is that function expressions are not hoisted (they have the same hoisting behaviour as variables). For more explanation on hoisting, refer to the question above on hoisting. If you try to invoke a function expression before it is defined, you will get an `Uncaught TypeError: XXX is not a function error`.
    Function Declaration
    
    ```
    foo(); // 'FOOOOO'
  function foo() {
    console.log('FOOOOO');
  }
  ```
  Function Expression
  
  ```
  foo(); // Uncaught TypeError: foo is not a function
  var foo = function() {
    console.log('FOOOOO');
  }
  ```
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function
  
* **What is an error-first callback?**

    Error-first callbacks are used to pass errors and data as well. You have to pass the error as the first parameter, and it has to be checked to see if something went wrong. Additional arguments are used to pass data.
    
    ```
    fs.readFile(filePath, function(err, data) {  
    if (err) {
      // handle the error, the return is important here
      // so execution stops here
      return console.log(err)
    }
    // use the data object
    console.log(data)
  })
  ```
  
* **How can you avoid callback hells?**

  There are lots of ways to solve the issue of callback hells:
  
  * modularization: break callbacks into independent functions
  * use a control flow library, like async
  * use generators with Promises
  * use async/await
  
* **What are Promises?**

    Promises are a concurrency primitive, first described in the 80s. Now they are part of most modern programming languages to make your life easier. Promises can help you better handle async operations.

    An example can be the following snippet, which after 100ms prints out the result string to the standard output. Also, note the catch, which can be used for error handling. Promises are chainable.
    
  ```
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('result')
    }, 100)
  })
    .then(console.log)
    .catch(console.error)
  ```
  
* **What tools can be used to assure consistent style? Why is it important?**

    When working in a team, consistent style is important, so team members can modify more projects easily, without having to get used to a new style each time.

    Also, it can help eliminate programming issues using static analysis.

    Tools that can help:
    
     * ESLint
     * Standard
     
     
* **What's a stub? Name a use case!**

    Stubs are functions/programs that simulate the behaviors of components/modules. Stubs provide canned answers to function calls made during test cases.

    An example can be writing a file, without actually doing so.
    
    ```
    var fs = require('fs')

    var writeFileStub = sinon.stub(fs, 'writeFile', function (path, data, cb) {  
      return cb(null)
    })

    expect(writeFileStub).to.be.called
    writeFileStub.restore()
    ```
    
* **What's a test pyramid? Give an example!**

    A test pyramid describes the ratio of how many unit tests, integration tests and end-to-end test you should write.
    
    An example for an HTTP API may look like this:

    * lots of low-level unit tests for models (dependencies are stubbed),
    * fewer integration tests, where you check how your models interact with each other (dependencies are not stubbed),
    * less end-to-end tests, where you call your actual endpoints (dependencies are not stubbed).
    
* **What's your favorite HTTP framework and why?**

    There is no right answer for this. The goal here is to understand how deeply one knows the framework she/he uses. Tell what are the pros and cons of picking that framework.
    
* **What is the purpose of cache busting and how can you achieve it?**

  Browsers have a cache to temporarily store files on websites so they don't need to be re-downloaded again when switching between pages or reloading the same page. The server is set up to send headers that tell the browser to store the file for a given amount of time. This greatly increases website speed and preserves bandwidth.

  However, it can cause problems when the website has been changed by developers because the user's cache still references old files. This can either leave them with old functionality or break a website if the cached CSS and JavaScript files are referencing elements that no longer exist, have moved or have been renamed.

  Cache busting is the process of forcing the browser to download the new files. This is done by naming the file something different to the old file.

  A common technique to force the browser to re-download the file is to append a query string to the end of the file.

  * `src="js/script.js"` => `src="js/script.js?v=2"`
  
  The browser considers it a different file but prevents the need to change the file name.
  
* **What is the difference between the postfix `i++` and prefix `++i` increment operators?**

    Both increment the variable value by 1. The difference is what they evaluate to.
    
    The postfix increment operator evaluates to the value before it was incremented.
    
    
    ```
    let i = 0
    i++ // 0
    // i === 1
    ```
    
    The prefix increment operator evaluates to the value after it was incremented.
    
    ```
    let i = 0
    ++i // 1
    // i === 1
    ````
    
* **In which states can a Promise be?**

  A `Promise` is in one of these states:

    *  pending: initial state, neither fulfilled nor rejected.
    * fulfilled: meaning that the operation completed successfully.
    * rejected: meaning that the operation failed.
    
    A pending promise can either be fulfilled with a value, or rejected with a reason (error). When either of these options happens, the associated handlers queued up by a promise's then method are called.
    
* **What are `defer` and `async` attributes on a `<script>` tag?**
  
    If neither attribute is present, the script is downloaded and executed synchronously, and will halt parsing of the document until it has finished executing (default behavior). Scripts are downloaded and executed in the order they are encountered.

    The `defer` attribute downloads the script while the document is still parsing but waits until the document has finished parsing before executing it, equivalent to executing inside a `DOMContentLoaded` event listener. `defer` scripts will execute in order.

    The `async` attribute downloads the script during parsing the document but will pause the parser to execute the script before it has fully finished parsing. `async` scripts will not necessarily execute in order.

    Note: both attributes must only be used if the script has a `src` attribute (i.e. not an inline script).
    
    ```
    <script src="myscript.js"></script>
    <script src="myscript.js" defer></script>
    <script src="myscript.js" async></script>
    ```
    
    **Good to hear**
    * Placing a `defer` script in the `<head>` allows the browser to download the script while the page is still parsing, and is therefore a better option than placing the script before the end of the body.

    * If the scripts rely on each other, use `defer`.

    * If the script is independent, use `async`.

    * Use `defer` if the DOM must be ready and the contents are not placed within a `DOMContentLoaded` listener.
    
* **What is a callback? Can you show an example using one?**

    Callbacks are functions passed as an argument to another function to be executed once an event has occurred or a certain task is complete, often used in asynchronous code. Callback functions are invoked later by a piece of code but can be declared on initialization without being invoked.

    As an example, event listeners are asynchronous callbacks that are only executed when a specific event occurs.
    
    ```
    function onClick() {
    console.log("The user clicked on the page.")
    }
    document.addEventListener("click", onClick)
    ```
    
    However, callbacks can also be synchronous. The following `map `function takes a callback function that is invoked synchronously for each iteration of the loop (array element).
    
    ```
    const map = (arr, callback) => {
      const result = []
      for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i], i))
      }
      return result
    }
    map([1, 2, 3, 4, 5], n => n * 2) // [2, 4, 6, 8, 10]
    ```
    
    **Good to hear**
    * Functions are first-class objects in JavaScript
    * Callbacks vs Promises
    
* **How do you clone an object in JavaScript?**

    Using the object spread operator `...`, the object's own enumerable properties can be copied into the new object. This creates a shallow clone of the object.
    
    ```
    const obj = { a: 1, b: 2 }
    const shallowClone = { ...obj }
    ```
    
    With this technique, prototypes are ignored. In addition, nested objects are not cloned, but rather their references get copied, so nested objects still refer to the same objects as the original. Deep-cloning is much more complex in order to effectively clone any type of object (Date, RegExp, Function, Set, etc) that may be nested within the object.
    
    Other alternatives include:

    * `JSON.parse(JSON.stringify(obj))` can be used to deep-clone a simple object, but it is CPU-intensive and only accepts valid JSON (therefore it strips functions and does not allow circular references).
    * `Object.assign({}, obj)` is another alternative.
    * `Object.keys(obj).reduce((acc, key) => (acc[key] = obj[key], acc), {})` is another more verbose alternative that shows the concept in greater depth.
    **Good to hear**
    * JavaScript passes objects by reference, meaning that nested objects get their references copied, instead of their values.
    * The same method can be used to merge two objects.
    
* **How do you compare two objects in JavaScript?**

    Even though two different objects can have the same properties with equal values, they are not considered equal when compared using `==` or `===`. This is because they are being compared by their reference (location in memory), unlike primitive values which are compared by value.
    
    In order to test if two objects are equal in structure, a helper function is required. It will iterate through the own properties of each object to test if they have the same values, including nested objects. Optionally, the prototypes of the objects may also be tested for equivalence by passing `true` as the 3rd argument.

    Note: this technique does not attempt to test equivalence of data structures other than plain objects, arrays, functions, dates and primitive values.
    
    ```
    function isDeepEqual(obj1, obj2, testPrototypes = false) {
      if (obj1 === obj2) {
        return true
      }

      if (typeof obj1 === "function" && typeof obj2 === "function") {
        return obj1.toString() === obj2.toString()
      }

      if (obj1 instanceof Date && obj2 instanceof Date) {
        return obj1.getTime() === obj2.getTime()
      }

      if (
        Object.prototype.toString.call(obj1) !==
          Object.prototype.toString.call(obj2) ||
        typeof obj1 !== "object"
      ) {
        return false
      }

      const prototypesAreEqual = testPrototypes
        ? isDeepEqual(
            Object.getPrototypeOf(obj1),
            Object.getPrototypeOf(obj2),
            true
          )
        : true

      const obj1Props = Object.getOwnPropertyNames(obj1)
      const obj2Props = Object.getOwnPropertyNames(obj2)

      return (
        obj1Props.length === obj2Props.length &&
        prototypesAreEqual &&
        obj1Props.every(prop => isDeepEqual(obj1[prop], obj2[prop]))
      )
    }
    ```
    
    **Good to hear**
    * Primitives like strings and numbers are compared by their value
    * Objects on the other hand are compared by their reference (location in memory)
    
* **What is CORS?**

    Cross-Origin Resource Sharing or CORS is a mechanism that uses additional HTTP headers to grant a browser permission to access resources from a server at an origin different from the website origin.

    An example of a cross-origin request is a web application served from `http://mydomain.com` that uses AJAX to make a request for `http://yourdomain.com`.

    For security reasons, browsers restrict cross-origin HTTP requests initiated by JavaScript. `XMLHttpRequest` and `fetch` follow the same-origin policy, meaning a web application using those APIs can only request HTTP resources from the same origin the application was accessed, unless the response from the other origin includes the correct CORS headers.

    **Good to hear**
    * CORS behavior is not an error,  it’s a security mechanism to protect users.
    * CORS is designed to prevent a malicious website that a user may unintentionally visit from making a request to a legitimate website to read their personal data or perform actions against their will.
    
* **What is the DOM?**

  The DOM (Document Object Model) is a cross-platform API that treats HTML and XML documents as a tree structure consisting of nodes. These nodes (such as elements and text nodes) are objects that can be programmatically manipulated and any visible changes made to them are reflected live in the document. In a browser, this API is available to JavaScript where DOM nodes can be manipulated to change their styles, contents, placement in the document, or interacted with through event listeners.
  
  **Good to hear**
  * The DOM was designed to be independent of any particular programming language, making the structural representation of the document available from a single, consistent API.

  * The DOM is constructed progressively in the browser as a page loads, which is why scripts are often placed at the bottom of a page, in the `<head>` with a `defer` attribute, or inside a `DOMContentLoaded` event listener. Scripts that manipulate DOM nodes should be run after the DOM has been constructed to avoid errors.

  * `document.getElementById()` and `document.querySelector()` are common functions for selecting DOM nodes.

  * Setting the `innerHTML` property to a new value runs the string through the HTML parser, offering an easy way to append dynamic HTML content to a node.
  
* **What is event delegation and why is it useful? Can you show an example of how to use it?**

    Event delegation is a technique of delegating events to a single common ancestor. Due to event bubbling, events "bubble" up the DOM tree by executing any handlers progressively on each ancestor element up to the root that may be listening to it.

    DOM events provide useful information about the element that initiated the event via `Event.target`. This allows the parent element to handle behavior as though the target element was listening to the event, rather than all children of the parent or the parent itself.

    This provides two main benefits:

    * It increases performance and reduces memory consumption by only needing to register a single event listener to handle potentially thousands of elements.
    * If elements are dynamically added to the parent, there is no need to register new event listeners for them.
    Instead of:
    
    ```
    document.querySelectorAll("button").forEach(button => {
    button.addEventListener("click", handleButtonClick)
    })
    ```
    
    Event delegation involves using a condition to ensure the child target matches our desired element:
    
    ```
    document.addEventListener("click", e => {
    if (e.target.closest("button")) {
      handleButtonClick()
      }
    })
    ```
    
    **Good to hear**
    * The difference between event bubbling and capturing
    
* **What will the console log in this example?**
    ```
    var foo = 1
    var foobar = function() {
      console.log(foo)
      var foo = 2
    }
    foobar()
    ```
    
    Due to hoisting, the local variable `foo` is declared before the `console.log` method is called. This means the local variable `foo` is passed as an argument to `console.log()` instead of the global one declared outside of the function. However, since the value is not hoisted with the variable declaration, the output will be `undefined`, not `2`.

    **Good to hear**
    * Hoisting is JavaScript’s default behavior of moving declarations to the top
    * Mention of `strict` mode
    
* **How does hoisting work in JavaScript?**
    
    Hoisting is a JavaScript mechanism where variable and function declarations are put into memory during the compile phase. This means that no matter where functions and variables are declared, they are moved to the top of their scope regardless of whether their scope is global or local.

    However, the value is not hoisted with the declaration.

    The following snippet:
    
    ```
    console.log(hoist)
    var hoist = "value"
    ```
    
    is equivalent to:
    
    ```
    var hoist
    console.log(hoist)
    hoist = "value"
    ```
    
    Therefore logging `hoist` outputs `undefined` to the console, not `"value"`.

    Hoisting also allows you to invoke a function declaration before it appears to be declared in a program.
    
    ```
    myFunction() // No error; logs "hello"
    function myFunction() {
      console.log("hello")
    }
    ```
    
    But be wary of function expressions that are assigned to a variable:
    
    ```
    myFunction() // Error: `myFunction` is not a function
    var myFunction = function() {
      console.log("hello")
    }
    ```
    
    **Good to hear**
    * Hoisting is JavaScript’s default behavior of moving declarations to the top
    * Functions declarations are hoisted before variable declarations
    
* **What is the reason for wrapping the entire contents of a JavaScript source file in a function that is immediately invoked?**

    This technique is very common in JavaScript libraries. It creates a closure around the entire contents of the file which creates a private namespace and thereby helps avoid potential name clashes between different JavaScript modules and libraries. The function is immediately invoked so that the namespace (library name) is assigned the return value of the function.
    
    ```
    const myLibrary = (function() {
      var privateVariable = 2
      return {
        publicMethod: () => privateVariable
      }
    })()
    privateVariable // ReferenceError
    myLibrary.publicMethod() // 2
  ```
  
  **Good to hear**
    * Used among many popular JavaScript libraries
    * Creates a private namespace
    
* **What is the difference between lexical scoping and dynamic scoping?**
  
    Lexical scoping refers to when the location of a function's definition determines which variables you have access to. On the other hand, dynamic scoping uses the location of the function's invocation to determine which variables are available.
    
    **Good to hear**
    * Lexical scoping is also known as static scoping.
    * Lexical scoping in JavaScript allows for the concept of closures.
    * Most languages use lexical scoping because it tends to promote source code that is more easily understood.
    
* **What is a MIME type and what is it used for?**

    `MIME` is an acronym for `Multi-purpose Internet Mail Extensions`. It is used as a standard way of classifying file types over the Internet.

    **Good to hear**
    * A `MIME type` actually has two parts: a type and a subtype that are separated by a slash (/). For example, the `MIME type` for Microsoft Word files is `application/msword` (i.e., type is application and the subtype is msword).
    
* **What is the difference between `null` and `undefined`?**

    In JavaScript, two values discretely represent nothing - `undefined` and `null`. The concrete difference between them is that `null` is explicit, while  `undefined` is implicit. When a property does not exist or a variable has not been given a value, the value is `undefined`. `null` is set as the value to explicitly indicate “no value”. In essence, `undefined` is used when the nothing is not known, and `null` is used when the nothing is known.

    **Good to hear**
    * `typeof undefined` evaluates to `"undefined"`.
    * `typeof null` evaluates `"object"`. However, it is still a primitive value and this is considered an implementation bug in JavaScript.
    * `undefined == null` evaluates to `true`.
    
* **Describe the different ways to create an object. When should certain ways be preferred over others?**

    **Object literal**
    
    Often used to store one occurrence of data.
    
    ```
    const person = {
      name: "John",
      age: 50,
      birthday() {
        this.age++
      }
    }
    person.birthday() // person.age === 51
    ```
    
    **Constructor**
    
    Often used when you need to create multiple instances of an object, each with their own data that other instances of the class cannot affect. The `new` operator must be used before invoking the constructor or the global object will be mutated.
    
     ```
     function Person(name, age) {
      this.name = name
      this.age = age
    }
    Person.prototype.birthday = function() {
      this.age++
    }
    const person1 = new Person("John", 50)
    const person2 = new Person("Sally", 20)
    person1.birthday() // person1.age === 51
    person2.birthday() // person2.age === 21
    ```
    
    **Factory function**
    
    Creates a new object similar to a constructor, but can store private data using a closure. There is also no need to use `new` before invoking the function or the `this` keyword. Factory functions usually discard the idea of prototypes and keep all properties and methods as own properties of the object.
    
    ```
    const createPerson = (name, age) => {
      const birthday = () => person.age++
      const person = { name, age, birthday }
      return person
    }
    const person = createPerson("John", 50)
    person.birthday() // person.age === 51
    ```
    
    **Object.create()**
    
    Sets the prototype of the newly created object.
    
    ```
    const personProto = {
      birthday() {
        this.age++
      }
    }
    const person = Object.create(personProto)
    person.age = 50
    person.birthday() // person.age === 51
    ```
    
    A second argument can also be supplied to `Object.create()` which acts as a descriptor for the new properties to be defined.
    
    ```
    Object.create(personProto, {
      age: {
        value: 50,
        writable: true,
        enumerable: true
      }
    })
    ```
    
    **Good to hear**
    * Prototypes are objects that other objects inherit properties and methods from.
    * Factory functions offer private properties and methods through a closure but increase memory usage as a tradeoff, while classes do not have private properties or methods but reduce memory impact by reusing a single prototype object.
    
* **What is the difference between a parameter and an argument?**

    Parameters are the variable names of the function definition, while arguments are the values given to a function when it is invoked.
    
    ```
    function myFunction(parameter1, parameter2) {
      console.log(arguments[0]) // "argument1"
    }
    myFunction("argument1", "argument2")
    ```
    
    **Good to hear**
    * `arguments` is an array-like object containing information about the arguments supplied to an invoked function.
    * `myFunction.length` describes the arity of a function (how many parameters it has, regardless of how many arguments it is supplied).
    
* **How does prototypal inheritance differ from classical inheritance?**
    
    In the classical inheritance paradigm, object instances inherit their properties and functions from a class, which acts as a blueprint for the object. Object instances are typically created using a constructor and the `new` keyword.

    In the prototypal inheritance paradigm, object instances inherit directly from other objects and are typically created using factory functions or `Object.create()`.
    
* **What is REST?**

    REST (REpresentational State Transfer) is a software design pattern for network architecture. A RESTful web application exposes data in the form of information about its resources.

    Generally, this concept is used in web applications to manage state. With most applications, there is a common theme of reading, creating, updating, and destroying data. Data is modularized into separate tables like `posts`, `users`, `comments`, and a RESTful API exposes access to this data with:

    * An identifier for the resource. This is known as the endpoint or URL for the resource.
    * The operation the server should perform on that resource in the form of an HTTP method or verb. The common HTTP methods are GET, POST, PUT, and DELETE.
    
    Here is an example of the URL and HTTP method with a posts resource:

    * Reading: `/posts/` => GET
    * Creating: `/posts/new` => POST
    * Updating: `/posts/:id` => PUT
    * Destroying: `/posts/:id` => DELETE
        
    **Good to hear**
    * Alternatives to this pattern like GraphQL
    
* **Are semicolons required in JavaScript?**
    
    Sometimes. Due to JavaScript's automatic semicolon insertion, the interpreter places semicolons after most statements. This means semicolons can be omitted in most cases.

    However, there are some cases where they are required. They are not required at the beginning of a block, but are if they follow a line and:
    
   1.  The line starts with `[`
   
   ```
   const previousLine = 3
   ;[1, 2, previousLine].map(n => n * 2)
   ```
   
   2. The line starts with `(`
   
   ```
   const previousLine = 3
   ;(function() {
     // ...
   })()
   ```
   
   In the above cases, the interpreter does not insert a semicolon after `3`, and therefore it will see the `3` as attempting object property access or being invoked as a function, which will throw errors.

    **Good to hear**
    * Semicolons are usually optional in JavaScript but have edge cases where they are required.
    * If you don't use semicolons, tools like Prettier will insert semicolons for you in the places where they are required on save in a text editor to prevent errors.
    
* **What is short-circuit evaluation in JavaScript?**

    Short-circuit evaluation involves logical operations evaluating from left-to-right and stopping early.
    
    ```true || false```
    
    In the above sample using logical OR, JavaScript won't look at the second operand `false`, because the expression evaluates to `true` regardless. This is known as short-circuit evaluation.

    This also works for logical AND.
    
    ```false && true```
    
    This means you can have an expression that throws an error if evaluated, and it won't cause issues.
    
    ```
    true || nonexistentFunction()
    false && nonexistentFunction()
    ```
    
    This remains true for multiple operations because of left-to-right evaluation.
    
    ```
    true || nonexistentFunction() || window.nothing.wouldThrowError
    true || window.nothing.wouldThrowError
    true
    ```
    
    A common use case for this behavior is setting default values. If the first operand is falsy the second operand will be evaluated.
    
    ```
    const options = {}
    const setting = options.setting || "default"
    setting // "default"
    ```
    
    Another common use case is only evaluating an expression if the first operand is truthy.
    
    ```
    // Instead of:
    addEventListener("click", e => {
      if (e.target.closest("button")) {
        handleButtonClick(e)
      }
    })

    // You can take advantage of short-circuit evaluation:
    addEventListener(
      "click",
      e => e.target.closest("button") && handleButtonClick(e)
    )
    ```
    
    In the above case, if `e.target` is not or does not contain an element matching the `"button"` selector, the function will not be called. This is because the first operand will be falsy, causing the second operand to not be evaluated.

    **Good to hear**
    * Logical operations do not produce a boolean unless the operand(s) evaluate to a boolean.
    
    
* **What is the difference between synchronous and asynchronous code in JavaScript?**

    Synchronous means each operation must wait for the previous one to complete.

    Asynchronous means an operation can occur while another operation is still being processed.

    In JavaScript, all code is synchronous due to the single-threaded nature of it. However, asynchronous operations not part of the program (such as `XMLHttpRequest` or `setTimeout`) are processed outside of the main thread because they are controlled by native code (browser APIs), but callbacks part of the program will still be executed synchronously.
    
    **Good to hear**
    * JavaScript has a concurrency model based on an "event loop".
    * Functions like `alert` block the main thread so that no user input is registered until the user closes it.
    
* **What does the following code evaluate to?**

    ```typeof typeof 0```
    
    It evaluates to `"string"`.

    `typeof 0` evaluates to the string `"number"` and therefore `typeof "number"` evaluates to `"string"`.
    
    
* **What are JavaScript data types?**

    The latest ECMAScript standard defines seven data types, six of them being primitive: `Boolean`, `Null`, `Undefined`, `Number`, `String`, `Symbol` and one non-primitive data type: `Object`.

    **Good to hear**
    * Mention of newly added `Symbol` data type
    * `Array`, `Date` and `function` are all of type `object`
    * Functions in JavaScript are objects with the capability of being callable
    
* **What are the differences between var, let, const and no keyword statements?**

    **No keyword**
    
    When no keyword exists before a variable assignment, it is either assigning a global variable if one does not exist, or reassigns an already declared variable. In non-strict mode, if the variable has not yet been declared, it will assign the variable as a property of the global object (`window` in browsers). In strict mode, it will throw an error to prevent unwanted global variables from being created.

    **var**
    
    `var` was the default statement to declare a variable until ES2015. It creates a function-scoped variable that can be reassigned and redeclared. However, due to its lack of block scoping, it can cause issues if the variable is being reused in a loop that contains an asynchronous callback because the variable will continue to exist outside of the block scope.

    Below, by the time the the `setTimeout` callback executes, the loop has already finished and the `i` variable is `10`, so all ten callbacks reference the same variable available in the function scope.
    
    ```
    for (var i = 0; i < 10; i++) {
      setTimeout(() => {
        // logs `10` ten times
        console.log(i)
      })
    }

    /* Solutions with `var` */
    for (var i = 0; i < 10; i++) {
      // Passed as an argument will use the value as-is in
      // that point in time
      setTimeout(console.log, 0, i)
    }

    for (var i = 0; i < 10; i++) {
      // Create a new function scope that will use the value
      // as-is in that point in time
      ;(i => {
        setTimeout(() => {
          console.log(i)
        })
      })(i)
    }
    ```
    
    **let**
    
    `let` was introduced in ES2015 and is the new preferred way to declare variables that will be reassigned later. Trying to redeclare a variable again will throw an error. It is block-scoped so that using it in a loop will keep it scoped to the iteration.
    
    ```
    for (let i = 0; i < 10; i++) {
      setTimeout(() => {
        // logs 0, 1, 2, 3, ...
        console.log(i)
      })
    }
    ```
    
    **const**
    
    `const` was introduced in ES2015 and is the new preferred default way to declare all variables if they won't be reassigned later, even for objects that will be mutated (as long as the reference to the object does not change). It is block-scoped and cannot be reassigned.
    
   ```
   const myObject = {}
   myObject.prop = "hello!" // No error
   myObject = "hello" // Error
   ```
   
    **Good to hear**
    * All declarations are hoisted to the top of their scope.

    * However, with `let` and `const` there is a concept called the temporal dead zone (TDZ). While the declarations are still hoisted, there is a period between entering scope and being declared where they cannot be accessed.

    * Show a common issue with using `var` and how `let` can solve it, as well as a solution that keeps `var`.

    * `var` should be avoided whenever possible and prefer `const` as the default declaration statement for all variables unless they will be reassigned later, then use `let` if so.
    
* **What is a cross-site scripting attack (XSS) and how do you prevent it?**

    XSS refers to client-side code injection where the attacker injects malicious scripts into a legitimate website or web application. This is often achieved when the application does not validate user input and freely injects dynamic HTML content.

    For example, a comment system will be at risk if it does not validate or escape user input. If the comment contains unescaped HTML, the comment can inject a `<script>` tag into the website that other users will execute against their knowledge.

    * The malicious script has access to cookies which are often used to store session tokens. If an attacker can obtain a user’s session cookie, they can impersonate the user.
    * The script can arbitrarily manipulate the DOM of the page the script is executing in, allowing the attacker to insert pieces of content that appear to be a real part of the website.
    * The script can use AJAX to send HTTP requests with arbitrary content to arbitrary destinations.
    
    **Good to hear**
    * On the client, using `textContent` instead of `innerHTML` prevents the browser from running the string through the HTML parser which would execute scripts in it.

    * On the server, escaping HTML tags will prevent the browser from parsing the user input as actual HTML and therefore won't execute the script.
    
* **What is Big O Notation?**

    Big O notation is used in Computer Science to describe the time complexity of an algorithm. The best algorithms will execute the fastest and have the simplest complexity.

    Algorithms don't always perform the same and may vary based on the data they are supplied. While in some cases they will execute quickly, in other cases they will execute slowly, even with the same number of elements to deal with.

    In these examples, the base time is 1 element = `1ms`.
    
    **O(1)**
    
    ```arr[arr.length - 1]```
    * 1000 elements = `1ms`
    
    Constant time complexity. No matter how many elements the array has, it will theoretically take (excluding real-world variation) the same amount of time to execute.
    
    **O(N)**
    
    ```arr.filter(fn)```
    * 1000 elements = `1000ms`
    
    Linear time complexity. The execution time will increase linearly with the number of elements the array has. If the array has 1000 elements and the function takes 1ms to execute, 7000 elements will take 7ms to execute. This is because the function must iterate through all elements of the array before returning a result.
    
    **O([1, N])**
    
    ```arr.some(fn)```
    * 1000 elements = `1ms <= x <= 1000ms`
    
    The execution time varies depending on the data supplied to the function, it may return very early or very late. The best case here is O(1) and the worst case is O(N).
    
    **O(NlogN)**
    
    ```arr.sort(fn)```
    * 1000 elements ~= `10000ms`
    
    Browsers usually implement the quicksort algorithm for the `sort()` method and the average time complexity of quicksort is O(NlgN). This is very efficient for large collections.
    
    **O(N^2)**
    
    ```
    for (let i = 0; i < arr.length; i++) {
      for (let j = 0; j < arr.length; j++) {
        // ...
      }
    }
    ```
    * 1000 elements = `1000000ms`
    
    The execution time rises quadratically with the number of elements. Usually the result of nesting loops.
    
    **O(N!)**
    
    ```
    const permutations = arr => {
      if (arr.length <= 2) return arr.length === 2 ? [arr, [arr[1], arr[0]]] : arr
      return arr.reduce(
        (acc, item, i) =>
          acc.concat(
            permutations([...arr.slice(0, i), ...arr.slice(i + 1)]).map(val => [
              item,
              ...val
            ])
          ),
        []
      )
    }
    ```
    
    * 1000 elements = `Infinity` (practically) ms
    
    The execution time rises extremely fast with even just 1 addition to the array.
    
    **Good to hear**
    * Be wary of nesting loops as execution time increases exponentially.
    
* **How can you avoid callback hells?**

    ```
    getData(function(a) {
      getMoreData(a, function(b) {
        getMoreData(b, function(c) {
          getMoreData(c, function(d) {
            getMoreData(d, function(e) {
              // ...
            })
          })
        })
      })
    })
    ```
    
    Refactoring the functions to return promises and using `async/await` is usually the best option. Instead of supplying the functions with callbacks that cause deep nesting, they return a promise that can be `await`ed and will be resolved once the data has arrived, allowing the next line of code to be evaluated in a sync-like fashion.

    The above code can be restructured like so:
    
    ```
    async function asyncAwaitVersion() {
    const a = await getData()
    const b = await getMoreData(a)
    const c = await getMoreData(b)
    const d = await getMoreData(c)
    const e = await getMoreData(d)
    // ...
    }
    ```
    
    There are lots of ways to solve the issue of callback hells:
    * Modularization: break callbacks into independent functions
    * Use a control flow library, like async
    * Use generators with Promises
    * Use async/await (from v7 on)

    **Good to hear**
    * As an efficient JavaScript developer, you have to avoid the constantly growing indentation level, produce clean and readable code and be able to handle complex flows.
    
* **What is a closure? Can you give a useful example of one?**

    A closure is a function defined inside another function and has access to its lexical scope even when it is executing outside its lexical scope. The closure has access to variables in three scopes:

    * Variables declared in its own scope
    * Variables declared in the scope of the parent function
    * Variables declared in the global scope
    
    In JavaScript, all functions are closures because they have access to the outer scope, but most functions don't utilise the usefulness of closures: the persistence of state. Closures are also sometimes called stateful functions because of this.

    In addition, closures are the only way to store private data that can't be accessed from the outside in JavaScript. They are the key to the UMD (Universal Module Definition) pattern, which is frequently used in libraries that only expose a public API but keep the implementation details private, preventing name collisions with other libraries or the user's own code.

    **Good to hear**
    * Closures are useful because they let you associate data with a function that operates on that data.
    * A closure can substitute an object with only a single method.
    * Closures can be used to emulate private properties and methods.
  
* **What is functional programming?**

    Functional programming is a paradigm in which programs are built in a declarative manner using pure functions that avoid shared state and mutable data. Functions that always return the same value for the same input and don't produce side effects are the pillar of functional programming. Many programmers consider this to be the best approach to software development as it reduces bugs and cognitive load.

    **Good to hear**
    * Cleaner, more concise development experience
    * Simple function composition
    * Features of JavaScript that enable functional programming (`.map`, `.reduce` etc.)
    * JavaScript is multi-paradigm programming language (Object-Oriented Programming and Functional Programming live in harmony)
    
* **What is HTML5 Web Storage? Explain `localStorage` and `sessionStorage`.**

    With HTML5, web pages can store data locally within the user’s browser. The data is stored in name/value pairs, and a web page can only access data stored by itself.

    **Differences between `localStorage` and `sessionStorage` regarding lifetime:**

    * Data stored through `localStorage` is permanent: it does not expire and remains stored on the user’s computer until a web app deletes it or the user asks the browser to delete it.
    * `sessionStorage` has the same lifetime as the top-level window or browser tab in which the data got stored. When the tab is permanently closed, any data stored through sessionStorage is deleted.
    
    **Differences between `localStorage` and `sessionStorage` regarding storage scope:** Both forms of storage are scoped to the document origin so that documents with different origins will never share the stored objects.

  * `sessionStorage` is also scoped on a per-window basis. Two browser tabs with documents from the same origin have separate `sessionStorage` data.
  * Unlike in `localStorage`, the same scripts from the same origin can't access each other's `sessionStorage` when opened in different tabs.
  
    **Good to hear**
    
    * Earlier, this was done with cookies.
    * The storage limit is far larger (at least 5MB) than with cookies and its faster.
    * The data is never transferred to the server and can only be used if the client specifically asks for it.
    
* **Explain the differences between imperative and declarative programming.**

    These two types of programming can roughly be summarized as:

    * Imperative: `how` to achieve something
    * Declarative: `what` should be achieved
    
    A common example of declarative programming is CSS. The developer specifies CSS properties that describe what something should look like rather than how to achieve it. The "how" is abstracted away by the browser.

    On the other hand, imperative programming involves the steps required to achieve something. In JavaScript, the differences can be contrasted like so:
    
    **Imperative**
    
    ```
    const numbers = [1, 2, 3, 4, 5]
    const numbersDoubled = []
    for (let i = 0; i < numbers.length; i++) {
      numbersDoubled[i] = numbers[i] * 2
    }
    ```
    
    We manually loop over the numbers of the array and assign the new index as the number doubled.
    
    **Declarative**
    
    ```
    const numbers = [1, 2, 3, 4, 5]
    const numbersDoubled = numbers.map(n => n * 2)
    ```
    
    We declare that the new array is mapped to a new one where each value is doubled.
    
    **Good to hear**
    * Declarative programming often works with functions and expressions. Imperative programming frequently uses statements and relies on low-level features that cause mutations, while declarative programming has a strong focus on abstraction and purity.
  * Declarative programming is more terse and easier to process at a glance.
  
* **What is memoization?**

    Memoization is the process of caching the output of function calls so that subsequent calls are faster. Calling the function again with the same input will return the cached output without needing to do the calculation again.

    A basic implementation in JavaScript looks like this:
    
    ```
    const memoize = fn => {
      const cache = new Map()
      return value => {
        const cachedResult = cache.get(value)
        if (cachedResult !== undefined) return cachedResult
        const result = fn(value)
        cache.set(value, result)
        return result
      }
    }
    ```
    
    **Good to hear**
    * The above technique returns a unary function even if the function can take multiple arguments.
    * The first function call will be slower than usual because of the overhead created by checking if a cached result exists and setting a result before returning the value.
    * Memoization increases performance on subsequent function calls but still needs to do work on the first call.
    
* **Contrast mutable and immutable values, and mutating vs non-mutating methods.**

    The two terms can be contrasted as:

    * Mutable: subject to change
    * Immutable: cannot change
    
    In JavaScript, objects are mutable while primitive values are immutable. This means operations performed on objects can change the original reference in some way, while operations performed on a primitive value cannot change the original value.

    All `String.prototype` methods do not have an effect on the original string and return a new string. On the other hand, while some methods of `Array.prototype` do not mutate the original array reference and produce a fresh array, some cause mutations.
    
    ```
    const myString = "hello!"
    myString.replace("!", "") // returns a new string, cannot mutate the original value

    const originalArray = [1, 2, 3]
    originalArray.push(4) // mutates originalArray, now [1, 2, 3, 4]
    originalArray.concat(4) // returns a new array, does not mutate the original
    ```

    **Good to hear**
    * List of mutating and non-mutating array methods
    
* **What is the only value not equal to itself in JavaScript?**

    `NaN` (Not-a-Number) is the only value not equal to itself when comparing with any of the comparison operators. `NaN` is often the result of meaningless math computations, so two `NaN` values make no sense to be considered equal.

    **Good to hear**
    * The difference between `isNaN()` and `Number.isNaN()`
    * `const isNaN = x => x !== x`
    
* **What is a pure function?**

    A pure function is a function that satisfies these two conditions:

    * Given the same input, the function returns the same output.
    * The function doesn't cause side effects outside of the function's scope (i.e. mutate data outside the function or data supplied to the function).
    
  Pure functions can mutate local data within the function as long as it satisfies the two conditions above.

  **Pure**
  
  ```
  const a = (x, y) => x + y
  const b = (arr, value) => arr.concat(value)
  const c = arr => [...arr].sort((a, b) => a - b)
  ```
  
  **Impure**
  
  ```
  const a = (x, y) => x + y + Math.random()
  const b = (arr, value) => (arr.push(value), arr)
  const c = arr => arr.sort((a, b) => a - b)
  ```
  
  **Good to hear**
  * Pure functions are easier to reason about due to their reliability.
  * All functions should be pure unless explicitly causing a side effect (i.e. setInnerHTML).
  * If a function does not return a value, it is an indication that it is causing side effects.
  
* **Explain the difference between a static method and an instance method.**

    Static methods belong to a class and don't act on instances, while instance methods belong to the class prototype which is inherited by all instances of the class and acts on them.
    
    ```
    Array.isArray // static method of Array
    Array.prototype.push // instance method of Array
    ```
    
    In this case, the `Array.isArray` method does not make sense as an instance method of arrays because we already know the value is an array when working with it.

    Instance methods could technically work as static methods, but provide terser syntax:
    
    ```
    const arr = [1, 2, 3]
    arr.push(4)
    Array.push(arr, 4)
    ```
    
    **Good to hear**  
    * How to create static and instance methods with ES2015 class syntax
    
* **What is the `this` keyword and how does it work?**

    The `this` keyword is an object that represents the context of an executing function. Regular functions can have their `this` value changed with the methods `call()`, `apply()` and `bind()`. Arrow functions implicitly `bind` this so that it refers to the context of its lexical environment, regardless of whether or not its context is set explicitly with `call()`.

    Here are some common examples of how `this` works:
    
    **Object literals**
    
    `this` refers to the object itself inside regular functions if the object precedes the invocation of the function.

    Properties set as `this` do not refer to the object.
    
    ```
      var myObject = {
        property: this,
      regularFunction: function() {
        return this
      },
      arrowFunction: () => {
        return this
      },
      iife: (function() {
        return this
      })()
    }
    myObject.regularFunction() // myObject
    myObject["regularFunction"]() // my Object

    myObject.property // NOT myObject; lexical `this`
    myObject.arrowFunction() // NOT myObject; lexical `this`
    myObject.iife // NOT myObject; lexical `this`
    const regularFunction = myObject.regularFunction
    regularFunction() // NOT myObject; lexical `this`
    ```
    **Event listeners**
    
    `this` refers to the element listening to the event.
    
    ```
    document.body.addEventListener("click", function() {
      console.log(this) // document.body
    })
    ```
    
    **Constructors**
    
    `this` refers to the newly created object.
    
    ```
    class Example {
      constructor() {
        console.log(this) // myExample
      }
    }
    const myExample = new Example()
    ```
    
    **Manual**
    
    With `call()` and `apply()`, `this` refers to the object passed as the first argument.
    
    ```
    var myFunction = function() {
      return this
    }
    myFunction.call({ customThis: true }) // { customThis: true }
    ```
    
    **Unwanted this**
    
    Because `this` can change depending on the scope, it can have unexpected values when using regular functions.
    
    ```
    var obj = {
      arr: [1, 2, 3],
      doubleArr() {
        return this.arr.map(function(value) {
          // this is now this.arr
          return this.double(value)
        })
      },
      double() {
        return value * 2
      }
    }
    obj.doubleArr() // Uncaught TypeError: this.double is not a function
    ```
    
    **Good to hear**
    * In non-strict mode, global `this` is the global object (`window` in browsers), while in strict mode global `this` is `undefined`.
    * `Function.prototype.call` and `Function.prototype.apply` set the `this` context of an executing function as the first argument, with `call` accepting a variadic number of arguments thereafter, and `apply` accepting an array as the second argument which are fed to the function in a variadic manner.
    * `Function.prototype.bind` returns a new function that enforces the `this` context as the first argument which cannot be changed by other functions.
    * If a function requires its `this` context to be changed based on how it is called, you must use the `function` keyword. Use arrow functions when you want `this` to be the surrounding (lexical) context.
    
* **What does `'use strict'` do and what are some of the key benefits to using it?**

    Including `'use strict'` at the beginning of your JavaScript source file enables strict mode, which enforces more strict parsing and error handling of JavaScript code. It is considered a good practice and offers a lot of benefits, such as:

    * Easier debugging due to eliminating silent errors.
    * Disallows variable redefinition.
    * Prevents accidental global variables.
    * Oftentimes provides increased performance over identical code that is not running in strict mode.
    * Simplifies `eval()` and `arguments`.
    * Helps make JavaScript more secure.
    
    **Good to hear**
    * Eliminates `this` coercion, throwing an error when `this` references a value of `null` or `undefined`.
    * Throws an error on invalid usage of `delete`.
    * Prohibits some syntax likely to be defined in future versions of ECMAScript
    
* **What is a potential pitfall with using `typeof bar === "object"` to determine if `bar` is an object? How can this pitfall be avoided?**

    Although `typeof bar === "object"` is a reliable way of checking if bar is an object, the surprising gotcha in JavaScript is that null is also considered an object!

    Therefore, the following code will, to the surprise of most developers, log true (not false) to the console:
    ```
    var bar = null;
    console.log(typeof bar === "object");  // logs true!
    ```
    As long as one is aware of this, the problem can easily be avoided by also checking if bar is null:

    ```console.log((bar !== null) && (typeof bar === "object"));  // logs false```
    To be entirely thorough in our answer, there are two other things worth noting:

    First, the above solution will return `false` if `bar` is a function. In most cases, this is the desired behavior, but in situations where you want to also return `true` for functions, you could amend the above solution to be:

    ```console.log((bar !== null) && ((typeof bar === "object") || (typeof bar === "function")));```
    
    Second, the above solution will return `true` if `bar` is an array (e.g., if `var bar = [];`). In most cases, this is the desired behavior, since arrays are indeed objects, but in situations where you want to also false for arrays, you could amend the above solution to be:

    ```console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"));```
    
    However, there’s one other alternative that returns false for nulls, arrays, and functions, but true for objects:

    ```console.log((bar !== null) && (bar.constructor === Object));```
    
    ES5 makes the array case quite simple, including its own null check:

    ```console.log(Array.isArray(bar));```
    
* **What will the code below output to the console and why?**

    ```
    (function(){
      var a = b = 3;
    })();

    console.log("a defined? " + (typeof a !== 'undefined'));
    console.log("b defined? " + (typeof b !== 'undefined'));
    ```
    
    Since both `a` and `b` are defined within the enclosing scope of the function, and since the line they are on begins with the `var` keyword, most JavaScript developers would expect `typeof a` and `typeof b` to both be undefined in the above example.

    However, that is *not* the case. The issue here is that most developers incorrectly understand the statement `var a = b = 3`; to be shorthand for:
    ```
    var b = 3;
    var a = b;
    ```
    But in fact, `var a = b = 3`; is actually shorthand for:
    ```
    b = 3;
    var a = b;
    ```
    As a result (if you are not using strict mode), the output of the code snippet would be:
    ```
    a defined? false
    b defined? true
    ```
    
    But how can `b` be defined outside of the scope of the enclosing function? Well, since the statement `var a = b = 3`; is shorthand for the statements `b = 3`; and `var a = b`;, `b` ends up being a global variable (since it is not preceded by the `var` keyword) and is therefore still in scope even outside of the enclosing function.

    Note that, in strict mode (i.e., with `use strict`), the statement `var a = b = 3`; will generate a runtime error of    `ReferenceError: b is not defined`, thereby avoiding any headfakes/bugs that might othewise result. (Yet another prime example of why you should use `use strict` as a matter of course in your code!)
    
* **What will the code below output to the console and why?**

    ```
    var myObject = {
      foo: "bar",
      func: function() {
          var self = this;
          console.log("outer func:  this.foo = " + this.foo);
          console.log("outer func:  self.foo = " + self.foo);
          (function() {
              console.log("inner func:  this.foo = " + this.foo);
              console.log("inner func:  self.foo = " + self.foo);
          }());
      }
    };
    myObject.func();
    ```
    
    The above code will output the following to the console:

    ```
    outer func:  this.foo = bar
    outer func:  self.foo = bar
    inner func:  this.foo = undefined
    inner func:  self.foo = bar
    ```
    
    In the outer function, both `this` and `self` refer to `myObject` and therefore both can properly reference and access `foo`.

    In the inner function, though, `this` no longer refers to `myObject`. As a result, `this.foo` is undefined in the inner function, whereas the reference to the local variable `self` remains in scope and is accessible there.
    
* **What is `NaN`? What is its type? How can you reliably test if a value is equal to `NaN`?**

    The `NaN` property represents a value that is “not a number”. This special value results from an operation that could not be performed either because one of the operands was non-numeric (e.g., `"abc" / 4`), or because the result of the operation is non-numeric.

    While this seems straightforward enough, there are a couple of somewhat surprising characteristics of NaN that can result in hair-pulling bugs if one is not aware of them.

    For one thing, although `NaN` means “not a number”, its type is, believe it or not, `Number`:

    ```console.log(typeof NaN === "number");  // logs "true"```
    
    Additionally, `NaN` compared to anything – even itself! – is false:

    ```console.log(NaN === NaN);  // logs "false"```
    
    A *semi-reliable* way to test whether a number is equal to NaN is with the built-in function `isNaN()`, but even using `isNaN()` is an imperfect solution.

    A better solution would either be to use `value !== value`, which would only produce true if the value is equal to NaN. Also, ES6 offers a new `Number.isNaN()` function, which is a different and more reliable than the old global `isNaN()` function.  
    
* **What will be the output of the following code:**

    ```
    for (var i = 0; i < 5; i++) {
      setTimeout(function() { console.log(i); }, i * 1000 );
    }
    ```
    
    The code sample shown will **not** display the values 0, 1, 2, 3, and 4 as might be expected; rather, it will display 5, 5, 5, 5, and 5.

    The reason for this is that each function executed within the loop will be executed after the entire loop has completed and all will therefore reference the last value stored in `i`, which was 5.

    Closures can be used to prevent this problem by creating a unique scope for each iteration, storing each unique value of the variable within its scope, as follows:
    
    ```
    for (var i = 0; i < 5; i++) {
      (function(x) {
          setTimeout(function() { console.log(x); }, x * 1000 );
      })(i);
    }
    ```
    
    This will produce the presumably desired result of logging 0, 1, 2, 3, and 4 to the console.

    In an ES2015 context, you can simply use `let` instead of `var` in the original code:
    
    ```
    for (let i = 0; i < 5; i++) {
      setTimeout(function() { console.log(i); }, i * 1000 );
    }
    ```
    
* **What would the following lines of code output to the console?**

    The code will output the following four lines:
    
    ```
    0 || 1 = 1
    1 || 2 = 1
    0 && 1 = 0
    1 && 2 = 2
    ```
    
    In JavaScript, both `||` and `&&` are logical operators that return the first fully-determined “logical value” when evaluated from left to right.
    
    **The or (`||`) operator**. In an expression of the form `X||Y`, `X` is first evaluated and interpreted as a boolean value. If this boolean value is `true`, then `true` (1) is returned and Y is not evaluated, since the “or” condition has already been satisfied. If this boolean value is “false”, though, we still don’t know if `X||Y` is true or false until we evaluate Y, and interpret it as a boolean value as well.

    Accordingly, `0 || 1` evaluates to true (1), as does `1 || 2`.

    **The and (&&) operator**. In an expression of the form `X&&Y`, `X` is first evaluated and interpreted as a boolean value. If this boolean value is `false`, then `false` (0) is returned and `Y` is not evaluated, since the “and” condition has already failed. If this boolean value is “true”, though, we still don’t know if `X&&Y` is true or false until we evaluate Y, and interpret it as a boolean value as well.

    However, the interesting thing with the `&&` operator is that when an expression is evaluated as “true”, then the expression itself is returned. This is fine, since it counts as “true” in logical expressions, but also can be used to return that value when you care to do so. This explains why, somewhat surprisingly, `1 && 2` returns 2 (whereas you might it expect it to return `true` or `1`).
    
* **What will be the output when the following code is executed? Explain.**

    ```
    console.log(false == '0')
    console.log(false === '0')
    ```
    
    The code will output:
    
    ```
    true
    false
    ```
    
    In JavaScript, there are two sets of equality operators. The triple-equal operator `===` behaves like any traditional equality operator would: evaluates to true if the two expressions on either of its sides have the same type and the same value. The double-equal operator, however, tries to coerce the values before comparing them. It is therefore generally good practice to use the `===` rather than `==`. The same holds true for `!==` vs `!=`.
    
* **What is the output out of the following code? Explain your answer.**

    ```
    var a={},
    b={key:'b'},
    c={key:'c'};

    a[b]=123;
    a[c]=456;

    console.log(a[b]);
    ```
    
    The output of this code will be `456` (not `123`).

    The reason for this is as follows: When setting an object property, JavaScript will implicitly **stringify** the parameter value. In this case, since `b` and `c` are both objects, they will both be converted to `"[object Object]"`. As a result, `a[b]` and `a[c]` are both equivalent to `a["[object Object]"]` and can be used interchangeably. Therefore, setting or referencing `a[c]` is precisely the same as setting or referencing `a[b]`.
    
* **What will the following code output to the console:**

    ```
    console.log((function f(n){return ((n > 1) ? n * f(n-1) : n)})(10));
    ```
    
    The code will output the value of 10 factorial (i.e., 10!, or 3,628,800).

    Here’s why:

    The named function `f()` calls itself recursively, until it gets down to calling `f(1)` which simply returns `1`. Here, therefore, is what this does:
    
    ```
    f(1): returns n, which is 1
    f(2): returns 2 * f(1), which is 2
    f(3): returns 3 * f(2), which is 6
    f(4): returns 4 * f(3), which is 24
    f(5): returns 5 * f(4), which is 120
    f(6): returns 6 * f(5), which is 720
    f(7): returns 7 * f(6), which is 5040
    f(8): returns 8 * f(7), which is 40320
    f(9): returns 9 * f(8), which is 362880
    f(10): returns 10 * f(9), which is 3628800
    ```
    
* **Consider the code snippet below. What will the console output be and why?**

    ```
    (function(x) {
      return (function(y) {
          console.log(x);
      })(2)
    })(1);
    ```
    
    The output will be `1`, even though the value of `x` is never set in the inner function. Here’s why:
  
    As explained in our JavaScript Hiring Guide, a **closure** is a function, along with all variables or functions that were in-scope at the time that the closure was created. In JavaScript, a closure is implemented as an “inner function”; i.e., a function defined within the body of another function. An important feature of closures is that an inner function still has access to the outer function’s variables.

    Therefore, in this example, since `x` is not defined in the inner function, the scope of the outer function is searched for a defined variable `x`, which is found to have a value of `1`.
    
* **What will the following code output to the console and why:**

    ```
    var hero = {
      _name: 'John Doe',
      getSecretIdentity: function (){
          return this._name;
      }
    };

    var stoleSecretIdentity = hero.getSecretIdentity;

    console.log(stoleSecretIdentity());
    console.log(hero.getSecretIdentity());
    ```
    
    **What is the issue with this code and how can it be fixed?**
    
    The code will output:
    ```
    undefined
    John Doe
    ```
    The first `console.log` prints `undefined` because we are extracting the method from the hero object, so `stoleSecretIdentity()` is being invoked in the global context (i.e., the window object) where the `_name` property does not exist.

    One way to fix the `stoleSecretIdentity()` function is as follows:

    ```var stoleSecretIdentity = hero.getSecretIdentity.bind(hero);```
    
* **Consider the following code. What will the output be, and why?**
    
    ```
    1
    undefined
    2
    ```
    
    `var` statements are hoisted (without their value initialization) to the top of the global or function scope it belongs to, even when it’s inside a `with` or `catch` block. However, the error’s identifier is only visible inside the `catch` block. It is equivalent to:
    
    ```
    (function () {
      var x, y; // outer and hoisted
      try {
          throw new Error();
      } catch (x /* inner */) {
          x = 1; // inner x, not the outer one
          y = 2; // there is only one y, which is in the outer scope
          console.log(x /* inner */);
      }
      console.log(x);
      console.log(y);
    })();
    ```
    
* **What will be the output of this code?**

    ```
    var x = 21;
    var girl = function () {
        console.log(x);
        var x = 20;
    };
    girl ();
    ```
    
    Neither 21, nor 20, the result is `undefined`

    It’s because JavaScript initialization is not hoisted.

    (Why doesn’t it show the global value of 21? The reason is that when the function is executed, it checks that there’s a local x variable present but doesn’t yet declare it, so it won’t look for global one.)
    
* **What do the following lines output, and why?**

    ```
    console.log(1 < 2 < 3);
    console.log(3 > 2 > 1);
    ```
    
    The first statement returns `true` which is as expected.

    The second returns `false` because of how the engine works regarding operator associativity for `<` and `>`. It compares left to right, so `3 > 2 > 1` JavaScript translates to `true > 1`. true has value `1`, so it then compares `1 > 1`, which is `false`.
    
* **How do you add an element at the begining of an array? How do you add one at the end?**

    ```
    var myArray = ['a', 'b', 'c', 'd'];
    myArray.push('end');
    myArray.unshift('start');
    console.log(myArray); // ["start", "a", "b", "c", "d", "end"]
    ```
    
    With ES6, one can use the spread operator:
    
    ```
    myArray = ['start', ...myArray];
    myArray = [...myArray, 'end'];
    ```
    
    Or, in short:
    
    ```
    myArray = ['start', ...myArray, 'end'];
    ```
    
* **What is the value of `typeof undefined == typeof NULL`?**

    The expression will be evaluated to true, since `NULL` will be treated as any other undefined variable.

    Note: JavaScript is case-sensitive and here we are using `NULL` instead of `null`.
    
* **What will the following code output and why?**

    ```
    var b = 1;
    function outer(){
        var b = 2
        function inner(){
            b++;
            var b = 3;
            console.log(b)
        }
        inner();
    }
    outer();
    ```
    
    Output to the console will be “3”.

    There are three closures in the example, each with it’s own `var b` declaration. When a variable is invoked closures will be checked in order from local to global until an instance is found. Since the `inner` closure has a b variable of its own, that is what will be output.

    Furthermore, due to hoisting the code in inner will be interpreted as follows:
    
    ```
    function inner () {
      var b; // b is undefined
      b++; // b is NaN
      b = 3; // b is 3
      console.log(b); // output "3"
    }
    ```
    
* **What is the difference between `undefined` and `not defined` in JavaScript?**

    In JavaScript, if you try to use a variable that doesn't exist and has not been declared, then JavaScript will throw an error `var name is not defined` and script will stop executing. However, if you use `typeof undeclared_variable`, then it will return `undefined`.

    Before getting further into this, let's first understand the difference between declaration and definition.

    Let's say `var x` is a declaration because you have not defined what value it holds yet, but you have declared its existence and the need for memory allocation.
    
    ```
    var x; // declaring x
    console.log(x); //output: undefined 
    ```
    
    Here `var x = 1` is both a declaration and definition (also we can say we are doing an initialisation). In the example above, the declaration and assignment of value happen inline for variable x. In JavaScript, every variable or function declaration you bring to the top of its current scope is called `hoisting`.

    The assignment happens in order, so when we try to access a variable that is declared but not defined yet, we will get the result `undefined`.
    
    ```
    var x; // Declaration
    if(typeof x === 'undefined') // Will return true
    ```
    
    If a variable that is neither declared nor defined, when we try to reference such a variable we'd get the result `not defined`
    
    ```
    console.log(y);  // Output: ReferenceError: y is not defined
    ```
    
* **What will be the output of the code below?**

    ```
    var y = 1;
    if (function f(){}) {
      y += typeof f;
    }
    console.log(y);
    ```
    
    The output would be `1undefined`. The if condition statement evaluates using `eval`, so `eval(function f(){})` returns`function f(){}` (which is true). Therefore, inside the `if` statement, executing `typeof f` returns `undefined` because the `if` statement code executes at run time, and the statement inside the `if` condition is evaluated during run time.
    
    ```
    var k = 1;
    if (1) {
      eval(function foo(){});
      k += typeof foo;
    }
    console.log(k); 
    ```
    
    The code above will also output `1undefined`
    
    ```
    var k = 1;
    if (1) {
      function foo(){};
      k += typeof foo;
    }
    console.log(k); // output 1function
    ```
    
* **What is the drawback of creating true private methods in JavaScript?**
    
    One of the drawbacks of creating true private methods in JavaScript is that they are very memory-inefficient, as a new copy of the method would be created for each instance.
    
    ```
    var Employee = function (name, company, salary) {
      this.name = name || "";       //Public attribute default value is null
      this.company = company || ""; //Public attribute default value is null
      this.salary = salary || 5000; //Public attribute default value is null

      // Private method
      var increaseSalary = function () {
          this.salary = this.salary + 1000;
      };

      // Public method
      this.dispalyIncreasedSalary = function() {
          increaseSlary();
          console.log(this.salary);
      };
    };

    // Create Employee class object
    var emp1 = new Employee("John","Pluto",3000);
    // Create Employee class object
    var emp2 = new Employee("Merry","Pluto",2000);
    // Create Employee class object
    var emp3 = new Employee("Ren","Pluto",2500);
    ```
    
    Here each instance variable `emp1`, `emp2`, `emp3` has its own copy of the `increaseSalary` private method.
    
    So, as a recommendation, don’t use private methods unless it’s necessary.
    
* **What will be the output of the following code?**
    ```
    var output = (function(x){
      delete x;
      return x;
    })(0);

    console.log(output);
    ```
    
    The output would be `0`. The `delete` operator is used to delete properties from an object. Here `x` is not an object but a local variable. `delete` operators don't affect local variables.
    
* **What will be the output of the following code?**
    
    ```
    var x = 1;
    var output = (function(){
     delete x;
      return x;
    })();
  
    console.log(output);
    ```
    
    
    The output would be `1`. The `delete` operator is used to delete the property of an object. Here `x` is not an object, but rather it's the global variable of type `number`.
    
* **What will be the output of the code below?**

    ```
    var x = { foo : 1};
    var output = (function(){
      delete x.foo;
      return x.foo;
   })();
  
  console.log(output);
  ```
  
  The output would be `undefined`. The `delete` operator is used to delete the property of an object. Here, `x` is an object which has the property `foo`, and as it is a self-invoking function, we will delete the `foo` property from object `x`. After doing so, when we try to reference a deleted property `foo`, the result is `undefined`.
  
* **What will be the output of the code below?**

    ```
    var Employee = {
      company: 'xyz'
    }
    var emp1 = Object.create(Employee);
    delete emp1.company
    console.log(emp1.company);
    ```
    
    The output would be `xyz`. Here, `emp1` object has `company` as its prototype property. The `delete` operator doesn't delete prototype property.

  `emp1` object doesn't have company as its own property. You can test it `console.log(emp1.hasOwnProperty('company')); //output : false`. However, we can delete the `company` property directly from the `Employee` object using `delete Employee.company`. Or, we can also delete the `emp1` object using the `__proto__` property `delete emp1.__proto__.company`.
  
* **What will be the output of the code below?**

    ```
    var trees = ["xyz","xxxx","test","ryan","apple"];
    delete trees[3];
  
    console.log(trees.length);
    ```
    
    The output would be `5`. When we use the `delete` operator to delete an array element, the array length is not affected from this. This holds even if you deleted all elements of an array using the `delete` operator.

    In other words, when the `delete` operator removes an array element, that deleted element is not longer present in array. In place of value at deleted index `undefined x 1` in chrome and `undefined` is placed at the index. If you do `console.log(trees)` output `["xyz", "xxxx", "test", undefined × 1, "apple"]` in Chrome and in Firefox `["xyz", "xxxx", "test", undefined, "apple"]`.
    
* **What is the difference between the function declarations below?**

    ```
    var foo = function(){ 
    // Some code
    }; 
    ```
    
    ```
    function bar(){ 
    // Some code
    }; 
    ```
    
    The main difference is the function `foo` is defined at `run-time` whereas function `bar` is defined at parse time. To understand this in better way, let's take a look at the code below:
    
    ```
    Run-Time function declaration 
    <script>
    foo(); // Calling foo function here will give an Error
      var foo = function(){ 
        console.log("Hi I am inside Foo");
     }; 
     </script>
    ```
    
    ```
    <script>
    Parse-Time function declaration 
    bar(); // Calling foo function will not give an Error
     function bar(){ 
      console.log("Hi I am inside Foo");
     }; 
    </script>
    ```
    
    Another advantage of this first-one way of declaration is that you can declare functions based on certain conditions. For example:

    ```
    <script>
    if(testCondition) {// If testCondition is true then 
       var foo = function(){ 
        console.log("inside Foo with testCondition True value");
       }; 
     }else{
       var foo = function(){ 
        console.log("inside Foo with testCondition false value");
       }; 
    }
    </script>
    ```
    
    However, if you try to run similar code using the format below, you'd get an error:
    
    ```
    <script>
    if(testCondition) {// If testCondition is true then 
       function foo(){ 
        console.log("inside Foo with testCondition True value");
       }; 
     }else{
       function foo(){ 
        console.log("inside Foo with testCondition false value");
       }; 
    }
    </script>
    ```
    
* **What is function hoisting in JavaScript?**

    Function Expression
    
    ```
    var foo = function foo(){ 
      return 12; 
    }; 
    ```
    
    In JavaScript, variable and functions are `hoisted`. Let's take function `hoisting` first. Basically, the JavaScript interpreter looks ahead to find all variable declarations and then hoists them to the top of the function where they're declared. For example:
    
    ```
    foo(); // Here foo is still undefined 
    var foo = function foo(){ 
      return 12; 
    }; 
    ```
    
    Behind the scene of the code above looks like this:
    
    ```
    var foo = undefined;
    foo(); // Here foo is undefined 
 	   foo = function foo(){
 	      / Some code stuff
      }
    ```
    
    ```
    var foo = undefined;
 	 foo = function foo(){
 	     / Some code stuff
    }
    foo(); // Now foo is defined here
    ```

* **What is the instanceof operator in JavaScript? What would be the output of the code below?**

    ```
    function foo(){ 
      return foo; 
    }
    new foo() instanceof foo;
    ```
    
    Here, `instanceof` operator checks the current object and returns true if the object is of the specified type.

    For Example:
    
    ```
    var dog = new Animal();
    dog instanceof Animal // Output : true
    ```
    
    Here `dog instanceof Animal` is true since `dog` inherits from `Animal.prototype`.
    
    ```
    var name = new String("xyz");
    name instanceof String // Output : true
    ```
      
    Here  `name instanceof String` is true since `dog` inherits from `String.prototype`. Now let's understand the code below:
        
    ```
    function foo(){ 
      return foo; 
    }
    new foo() instanceof foo;
    ```
    
    Here function `foo` is returning `foo`, which again points to function `foo`.
    
    ```
    function foo(){
      return foo; 
    }
    var bar = new foo();
    // here bar is pointer to function foo(){return foo}.
    ```
    
    So the `new foo() instanceof foo` return `false`;
    
* **Explain event delegation.**
* **Explain how `this` works in JavaScript.**
    * Can you give an example of one of the ways that working with `this` has changed in ES6?
* **Explain how prototypal inheritance works.**
* **What's the difference between a variable that is: `null`, `undefined` or undeclared?**
    * How would you go about checking for any of these states?
* **What is a closure, and how/why would you use one?**
    A closure is the combination of a function and the lexical environment within which that function was declared. The word "lexical" refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Closures are functions that have access to the outer (enclosing) function's variables—scope chain even after the outer function has returned.
    **Why would you use one?**
      * Data privacy / emulating private methods with closures. Commonly used in the module pattern.
* **What language constructions do you use for iterating over object properties and array items?**
* **Can you describe the main difference between the `Array.forEach()` loop and `Array.map()` methods and why you would pick one versus the other?**
     ```forEach```
     * Iterates through the elements in an array.
     * Executes a callback for each element.
     * Does not return a value.
    ```
    const a = [1, 2, 3];
    const doubled = a.forEach((num, index) => {
      // Do something with num and/or index.
    });

    // doubled = undefined
    ```
    ```
    map
    ```
    * Iterates through the elements in an array.
    * "Maps" each element to a new element by calling the function on each element, creating a new array as a result.
    ```
    const a = [1, 2, 3];
    const doubled = a.map(num => {
      return num * 2;
    });

    // doubled = [2, 4, 6]
    ```
    
    The main difference between `.forEach` and `.map()` is that `.map()` returns a new array. If you need the result, but do not wish to mutate the original array, `.map()` is the clear choice. If you simply need to iterate over an array, `forEach` is a fine choice.
* **What's a typical use case for anonymous functions?**
    They can be used in IIFEs to encapsulate some code within a local scope so that variables declared in it do not leak to the global scope.
    ```
    (function() {
      // Some code here.
    })();
    ```
    As a callback that is used once and does not need to be used anywhere else. The code will seem more self-contained and readable when handlers are defined right inside the code calling them, rather than having to search elsewhere to find the function body.
    ```
    setTimeout(function() {
      console.log('Hello world!');
    }, 1000);
    ```
    Arguments to functional programming constructs or Lodash (similar to callbacks).
    ```
    const arr = [1, 2, 3];
    const double = arr.map(function(el) {
      return el * 2;
    });
    console.log(double); // [2, 4, 6]
    ```
* **What's the difference between host objects and native objects?**
    Native objects are objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.
     Host objects are provided by the runtime environment (browser or Node), such as `window`, `XMLHTTPRequest`, etc.
* **Explain the difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?**
* **Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`**
* **Can you explain what `Function.call` and `Function.apply` do? What's the notable difference between the two?**
* **Explain `Function.prototype.bind`.**
* **What's the difference between feature detection, feature inference, and using the UA string?**
   
   **Feature Detection**
    
    Feature detection involves working out whether a browser supports a certain block of code, and running different code depending on whether it does (or doesn't), so that the browser can always provide a working experience rather crashing/erroring in some browsers. For example:
    ```
    if ('geolocation' in navigator) {
      // Can use navigator.geolocation
    } else {
      // Handle lack of feature
    }
    ```
    Modernizr is a great library to handle feature detection.
    
    **Feature Inference**
    
    Feature inference checks for a feature just like feature detection, but uses another function because it assumes it will also exist, e.g.:
    
    ```
    if (document.getElementsByTagName) {
      element = document.getElementById(id);
    }
    ```
    This is not really recommended. Feature detection is more foolproof.
    
    **UA String**
    
    This is a browser-reported string that allows the network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent. It can be accessed via `navigator.userAgent`. However, the string is tricky to parse and can be spoofed. For example, Chrome reports both as Chrome and Safari. So to detect Safari you have to check for the Safari string and the absence of the Chrome string. Avoid this method.
* **Explain "hoisting".**
    Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the `var` keyword will have their declaration "moved" up to the top of the current scope, which we refer to as hoisting. However, only the declaration is hoisted, the assignment (if there is one), will stay where it is.

    Note that the declaration is not actually moved - the JavaScript engine parses the declarations during compilation and becomes aware of declarations and their scopes. It is just easier to understand this behavior by visualizing the declarations as being hoisted to the top of their scope. Let's explain with a few examples.
    ```
    // var declarations are hoisted.
    console.log(foo); // undefined
    var foo = 1;
    console.log(foo); // 1

    // let/const declarations are NOT hoisted.
    console.log(bar); // ReferenceError: bar is not defined
    let bar = 2;
    console.log(bar); // 2
    ```
    Function declarations have the body hoisted while the function expressions (written in the form of variable declarations) only has the variable declaration hoisted.
    ```
    // Function Declaration
    console.log(foo); // [Function: foo]
    foo(); // 'FOOOOO'
    function foo() {
      console.log('FOOOOO');
    }
    console.log(foo); // [Function: foo]

    // Function Expression
    console.log(bar); // undefined
    bar(); // Uncaught TypeError: bar is not a function
    var bar = function() {
      console.log('BARRRR');
    };
    console.log(bar); // [Function: bar]
    ```
* **Describe event bubbling.**
    When an event triggers on a DOM element, it will attempt to handle the event if there is a listener attached, then the event is bubbled up to its parent and the same thing happens. This bubbling occurs up the element's ancestors all the way to the `document`. Event bubbling is the mechanism behind event delegation.
* **Describe event capturing.**
* **What's the difference between an "attribute" and a "property"?**
    Attributes are defined on the HTML markup but properties are defined on the DOM. To illustrate the difference, imagine we have this text field in our HTML: `<input type="text" value="Hello">`.
    ```
    const input = document.querySelector('input');
    console.log(input.getAttribute('value')); // Hello
    console.log(input.value); // Hello
    ```
    But after you change the value of the text field by adding "World!" to it, this becomes:
    ```
    console.log(input.getAttribute('value')); // Hello
    console.log(input.value); // Hello World!
    ```
* **What are the pros and cons of extending built-in JavaScript objects?**
* **Explain the same-origin policy with regards to JavaScript.**
    The same-origin policy prevents JavaScript from making requests across domain boundaries. An origin is defined as a combination of URI scheme, hostname, and port number. This policy prevents a malicious script on one page from obtaining access to sensitive data on another web page through that page's Document Object Model.
* **Why is it called a Ternary operator, what does the word "Ternary" indicate?**
* **What is strict mode? What are some of the advantages/disadvantages of using it?**
* **What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?**
* **What tools and techniques do you use debugging JavaScript code?**
* **Explain the difference between mutable and immutable objects.**
    * What is an example of an immutable object in JavaScript?
    * What are the pros and cons of immutability?
    * How can you achieve immutability in your own code?
* **Explain the difference between synchronous and asynchronous functions.**
* **What is event loop?**
* **What is the difference between call stack and task queue?**
* **What are the differences between variables created using `let`, `var` or `const`?**
* **What are the differences between ES6 class and ES5 function constructors?**
* **Can you offer a use case for the new arrow `=>` function syntax? How does this new syntax differ from other functions?**
* **What advantage is there for using the arrow syntax for a method in a constructor?**
* **What is the definition of a higher-order function?**
* **Can you give an example for destructuring an object or an array?**
* **Can you give an example of generating a string with ES6 Template Literals?**
* **Can you give an example of a curry function and why this syntax offers an advantage?**
* **What are the benefits of using `spread syntax` and how is it different from `rest syntax`?**
* **How can you share code between files?**
* **Why you might want to create static class members?**
* **Explain how JSONP works (and how it's not really Ajax).**
    JSONP (JSON with Padding) is a method commonly used to bypass the cross-domain policies in web browsers because Ajax requests from the current page to a cross-origin domain is not allowed.
    JSONP works by making a request to a cross-origin domain via a `<script>` tag and usually with a `callback` query parameter, for example: `https://example.com?callback=printData`. The server will then wrap the data within a function called `printData` and return it to the client.
    ```
    <!-- https://mydomain.com -->
    <script>
    function printData(data) {
      console.log(`My name is ${data.name}!`);
    }
    </script>

    <script src="https://example.com?callback=printData"></script>
    ```
    ```
    // File loaded from https://example.com?callback=printData
    printData({ name: 'Yang Shun' });
    ```
    The client has to have the `printData` function in its global scope and the function will be executed by the client when the response from the cross-origin domain is received.

  JSONP can be unsafe and has some security implications. As JSONP is really JavaScript, it can do everything else JavaScript can do, so you need to trust the provider of the JSONP data.

  These days, CORS is the recommended approach and JSONP is seen as a hack.
  
* **Why is extending built-in JavaScript objects not a good idea?**
    Extending a built-in/native JavaScript object means adding properties/functions to its `prototype`. While this may seem like a good idea at first, it is dangerous in practice. Imagine your code uses a few libraries that both extend the `Array.prototype` by adding the same `contains`  method, the implementations will overwrite each other and your code will break if the behavior of these two methods is not the same.

    The only time you may want to extend a native object is when you want to create a polyfill, essentially providing your own implementation for a method that is part of the JavaScript specification but might not exist in the user's browser due to it being an older browser.
    
* **Difference between document `load` event and document` DOMContentLoaded` event**
    The `DOMContentLoaded` event is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

    `window`'s `load` event is only fired after the DOM and all dependent resources and assets have loaded.
    
* **Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?**

    Every script has access to the global scope, and if everyone uses the global namespace to define their variables, collisions will likely occur. Use the module pattern (IIFEs) to encapsulate your variables within a local namespace.
    
    # List of (Advanced) JavaScript Questions

###### 1. What's the output?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

- A: `Lydia` and `undefined`
- B: `Lydia` and `ReferenceError`
- C: `ReferenceError` and `21`
- D: `undefined` and `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

Within the function, we first declare the `name` variable with the `var` keyword. This means that the variable gets hoisted (memory space is set up during the creation phase) with the default value of `undefined`, until we actually get to the line where we define the variable. We haven't defined the variable yet on the line where we try to log the `name` variable, so it still holds the value of `undefined`.

Variables with the `let` keyword (and `const`) are hoisted, but unlike `var`, don't get <i>initialized</i>. They are not accessible before the line we declare (initialize) them. This is called the "temporal dead zone". When we try to access the variables before they are declared, JavaScript throws a `ReferenceError`.

</p>
</details>

---

###### 2. What's the output?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` and `0 1 2`
- B: `0 1 2` and `3 3 3`
- C: `3 3 3` and `0 1 2`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Because of the event queue in JavaScript, the `setTimeout` callback function is called _after_ the loop has been executed. Since the variable `i` in the first loop was declared using the `var` keyword, this value was global. During the loop, we incremented the value of `i` by `1` each time, using the unary operator `++`. By the time the `setTimeout` callback function was invoked, `i` was equal to `3` in the first example.

In the second loop, the variable `i` was declared using the `let` keyword: variables declared with the `let` (and `const`) keyword are block-scoped (a block is anything between `{ }`). During each iteration, `i` will have a new value, and each value is scoped inside the loop.

</p>
</details>

---

###### 3. What's the output?

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

console.log(shape.diameter());
console.log(shape.perimeter());
```

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Note that the value of `diameter` is a regular function, whereas the value of `perimeter` is an arrow function.

With arrow functions, the `this` keyword refers to its current surrounding scope, unlike regular functions! This means that when we call `perimeter`, it doesn't refer to the shape object, but to its surrounding scope (window for example).

There is no value `radius` on that object, which returns `undefined`.

</p>
</details>

---

###### 4. What's the output?

```javascript
+true;
!"Lydia";
```

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The unary plus tries to convert an operand to a number. `true` is `1`, and `false` is `0`.

The string `'Lydia'` is a truthy value. What we're actually asking, is "is this truthy value falsy?". This returns `false`.

</p>
</details>

---

###### 5. Which one is true?

```javascript
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

- A: `mouse.bird.size` is not valid
- B: `mouse[bird.size]` is not valid
- C: `mouse[bird["size"]]` is not valid
- D: All of them are valid

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

In JavaScript, all object keys are strings (unless it's a Symbol). Even though we might not _type_ them as strings, they are always converted into strings under the hood.

JavaScript interprets (or unboxes) statements. When we use bracket notation, it sees the first opening bracket `[` and keeps going until it finds the closing bracket `]`. Only then, it will evaluate the statement.

`mouse[bird.size]`: First it evaluates `bird.size`, which is `"small"`. `mouse["small"]` returns `true`

However, with dot notation, this doesn't happen. `mouse` does not have a key called `bird`, which means that `mouse.bird` is `undefined`. Then, we ask for the `size` using dot notation: `mouse.bird.size`. Since `mouse.bird` is `undefined`, we're actually asking `undefined.size`. This isn't valid, and will throw an error similar to `Cannot read property "size" of undefined`.

</p>
</details>

---


###### 6. What's the output?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

In JavaScript, all objects interact by _reference_ when setting them equal to each other.

First, variable `c` holds a value to an object. Later, we assign `d` with the same reference that `c` has to the object.

<img src="https://i.imgur.com/ko5k0fs.png" width="200">

When you change one object, you change all of them.

</p>
</details>

---

###### 7. What's the output?

```javascript
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`new Number()` is a built-in function constructor. Although it looks like a number, it's not really a number: it has a bunch of extra features and is an object.

When we use the `==` operator, it only checks whether it has the same _value_. They both have the value of `3`, so it returns `true`.

However, when we use the `===` operator, both value _and_ type should be the same. It's not: `new Number()` is not a number, it's an **object**. Both return `false.`

</p>
</details>

---

###### 8. What's the output?

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
console.log(freddie.colorChange("orange"));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The `colorChange` function is static. Static methods are designed to live only on the constructor in which they are created, and cannot be passed down to any children. Since `freddie` is a child, the function is not passed down, and not available on the `freddie` instance: a `TypeError` is thrown.

</p>
</details>

---

###### 9. What's the output?

```javascript
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

It logs the object, because we just created an empty object on the global object! When we mistyped `greeting` as `greetign`, the JS interpreter actually saw this as `global.greetign = {}` (or `window.greetign = {}` in a browser).

In order to avoid this, we can use `"use strict"`. This makes sure that you have declared a variable before setting it equal to anything.

</p>
</details>

---

###### 10. What happens when we do this?

```javascript
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- A: Nothing, this is totally fine!
- B: `SyntaxError`. You cannot add properties to a function this way.
- C: `"Woof"` gets logged.
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

This is possible in JavaScript, because functions are objects! (Everything besides primitive types are objects)

A function is a special type of object. The code you write yourself isn't the actual function. The function is an object with properties. This property is invocable.

</p>
</details>

---

###### 11. What's the output?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

You can't add properties to a constructor like you can with regular objects. If you want to add a feature to all objects at once, you have to use the prototype instead. So in this case,

```js
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

would have made `member.getFullName()` work. Why is this beneficial? Say that we added this method to the constructor itself. Maybe not every `Person` instance needed this method. This would waste a lot of memory space, since they would still have that property, which takes of memory space for each instance. Instead, if we only add it to the prototype, we just have it at one spot in memory, yet they all have access to it!

</p>
</details>

---

###### 12. What's the output?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` and `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` and `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` and `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` and `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

For `sarah`, we didn't use the `new` keyword. When using `new`, it refers to the new empty object we create. However, if you don't add `new` it refers to the **global object**!

We said that `this.firstName` equals `"Sarah"` and `this.lastName` equals `"Smith"`. What we actually did, is defining `global.firstName = 'Sarah'` and `global.lastName = 'Smith'`. `sarah` itself is left `undefined`, since we don't return a value from the `Person` function.

</p>
</details>

---

###### 13. What are the three phases of event propagation?

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

During the **capturing** phase, the event goes through the ancestor elements down to the target element. It then reaches the **target** element, and **bubbling** begins.

<img src="https://i.imgur.com/N18oRgd.png" width="200">

</p>
</details>

---

###### 14. All object have prototypes.

- A: true
- B: false

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

All objects have prototypes, except for the **base object**. The base object is the object created by the user, or an object that is created using the `new` keyword. The base object has access to some methods and properties, such as `.toString`. This is the reason why you can use built-in JavaScript methods! All of such methods are available on the prototype. Although JavaScript can't find it directly on your object, it goes down the prototype chain and finds it there, which makes it accessible for you.

</p>
</details>

---

###### 15. What's the output?

```javascript
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

- A: `NaN`
- B: `TypeError`
- C: `"12"`
- D: `3`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

JavaScript is a **dynamically typed language**: we don't specify what types certain variables are. Values can automatically be converted into another type without you knowing, which is called _implicit type coercion_. **Coercion** is converting from one type into another.

In this example, JavaScript converts the number `1` into a string, in order for the function to make sense and return a value. During the addition of a numeric type (`1`) and a string type (`'2'`), the number is treated as a string. We can concatenate strings like `"Hello" + "World"`, so what's happening here is `"1" + "2"` which returns `"12"`.

</p>
</details>

---

###### 16. What's the output?

```javascript
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- A: `1` `1` `2`
- B: `1` `2` `2`
- C: `0` `2` `2`
- D: `0` `1` `2`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The **postfix** unary operator `++`:

1. Returns the value (this returns `0`)
2. Increments the value (number is now `1`)

The **prefix** unary operator `++`:

1. Increments the value (number is now `2`)
2. Returns the value (this returns `2`)

This returns `0 2 2`.

</p>
</details>

---

###### 17. What's the output?

```javascript
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = "Lydia";
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

If you use tagged template literals, the value of the first argument is always an array of the string values. The remaining arguments get the values of the passed expressions!

</p>
</details>

---

###### 18. What's the output?

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

When testing equality, primitives are compared by their _value_, while objects are compared by their _reference_. JavaScript checks if the objects have a reference to the same location in memory.

The two objects that we are comparing don't have that: the object we passed as a parameter refers to a different location in memory than the object we used in order to check equality.

This is why both `{ age: 18 } === { age: 18 }` and `{ age: 18 } == { age: 18 }` return `false`.

</p>
</details>

---

###### 19. What's the output?

```javascript
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The rest parameter (`...args`.) lets us "collect" all remaining arguments into an array. An array is an object, so `typeof args` returns `"object"`

</p>
</details>

---

###### 20. What's the output?

```javascript
function getAge() {
  "use strict";
  age = 21;
  console.log(age);
}

getAge();
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

With `"use strict"`, you can make sure that you don't accidentally declare global variables. We never declared the variable `age`, and since we use `"use strict"`, it will throw a reference error. If we didn't use `"use strict"`, it would have worked, since the property `age` would have gotten added to the global object.

</p>
</details>

---

###### 21. What's value of `sum`?

```javascript
const sum = eval("10*10+5");
```

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

`eval` evaluates codes that's passed as a string. If it's an expression, like in this case, it evaluates the expression. The expression is `10 * 10 + 5`. This returns the number `105`.

</p>
</details>

---

###### 22. How long is cool_secret accessible?

```javascript
sessionStorage.setItem("cool_secret", 123);
```

- A: Forever, the data doesn't get lost.
- B: When the user closes the tab.
- C: When the user closes the entire browser, not only the tab.
- D: When the user shuts off their computer.

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The data stored in `sessionStorage` is removed after closing the _tab_.

If you used `localStorage`, the data would've been there forever, unless for example `localStorage.clear()` is invoked.

</p>
</details>

---

###### 23. What's the output?

```javascript
var num = 8;
var num = 10;

console.log(num);
```

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

With the `var` keyword, you can declare multiple variables with the same name. The variable will then hold the latest value.

You cannot do this with `let` or `const` since they're block-scoped.

</p>
</details>

---

###### 24. What's the output?

```javascript
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

All object keys (excluding Symbols) are strings under the hood, even if you don't type it yourself as a string. This is why `obj.hasOwnProperty('1')` also returns true.

It doesn't work that way for a set. There is no `'1'` in our set: `set.has('1')` returns `false`. It has the numeric type `1`, `set.has(1)` returns `true`.

</p>
</details>

---

###### 25. What's the output?

```javascript
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

If you have two keys with the same name, the key will be replaced. It will still be in its first position, but with the last specified value.

</p>
</details>

---

###### 26. The JavaScript global execution context creates two things for you: the global object, and the "this" keyword.

- A: true
- B: false
- C: it depends

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The base execution context is the global execution context: it's what's accessible everywhere in your code.

</p>
</details>

---

###### 27. What's the output?

```javascript
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- A: `1` `2`
- B: `1` `2` `3`
- C: `1` `2` `4`
- D: `1` `3` `4`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The `continue` statement skips an iteration if a certain condition returns `true`.

</p>
</details>

---

###### 28. What's the output?

```javascript
String.prototype.giveLydiaPizza = () => {
  return "Just give Lydia pizza already!";
};

const name = "Lydia";

name.giveLydiaPizza();
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

`String` is a built-in constructor, which we can add properties to. I just added a method to its prototype. Primitive strings are automatically converted into a string object, generated by the string prototype function. So, all strings (string objects) have access to that method!

</p>
</details>

---

###### 29. What's the output?

```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Object keys are automatically converted into strings. We are trying to set an object as a key to object `a`, with the value of `123`.

However, when we stringify an object, it becomes `"[Object object]"`. So what we are saying here, is that `a["Object object"] = 123`. Then, we can try to do the same again. `c` is another object that we are implicitly stringifying. So then, `a["Object object"] = 456`.

Then, we log `a[b]`, which is actually `a["Object object"]`. We just set that to `456`, so it returns `456`.

</p>
</details>

---

###### 30. What's the output?

```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
```

- A: `First` `Second` `Third`
- B: `First` `Third` `Second`
- C: `Second` `First` `Third`
- D: `Second` `Third` `First`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

We have a `setTimeout` function and invoked it first. Yet, it was logged last.

This is because in browsers, we don't just have the runtime engine, we also have something called a `WebAPI`. The `WebAPI` gives us the `setTimeout` function to start with, and for example the DOM.

After the _callback_ is pushed to the WebAPI, the `setTimeout` function itself (but not the callback!) is popped off the stack.

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

Now, `foo` gets invoked, and `"First"` is being logged.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo` is popped off the stack, and `baz` gets invoked. `"Third"` gets logged.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

The WebAPI can't just add stuff to the stack whenever it's ready. Instead, it pushes the callback function to something called the _queue_.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

This is where an event loop starts to work. An **event loop** looks at the stack and task queue. If the stack is empty, it takes the first thing on the queue and pushes it onto the stack.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

`bar` gets invoked, `"Second"` gets logged, and it's popped off the stack.

</p>
</details>

---

###### 31. What is the event.target when clicking the button?

```html
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Outer `div`
- B: Inner `div`
- C: `button`
- D: An array of all nested elements.

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The deepest nested element that caused the event is the target of the event. You can stop bubbling by `event.stopPropagation`

</p>
</details>

---

###### 32. When you click the paragraph, what's the logged output?

```html
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

- A: `p` `div`
- B: `div` `p`
- C: `p`
- D: `div`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

If we click `p`, we see two logs: `p` and `div`. During event propagation, there are 3 phases: capturing, target, and bubbling. By default, event handlers are executed in the bubbling phase (unless you set `useCapture` to `true`). It goes from the deepest nested element outwards.

</p>
</details>

---

###### 33. What's the output?

```javascript
const person = { name: "Lydia" };

function sayHi(age) {
  console.log(`${this.name} is ${age}`);
}

sayHi.call(person, 21);
sayHi.bind(person, 21);
```

- A: `undefined is 21` `Lydia is 21`
- B: `function` `function`
- C: `Lydia is 21` `Lydia is 21`
- D: `Lydia is 21` `function`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

With both, we can pass the object to which we want the `this` keyword to refer to. However, `.call` is also _executed immediately_!

`.bind.` returns a _copy_ of the function, but with a bound context! It is not executed immediately.

</p>
</details>

---

###### 34. What's the output?

```javascript
function sayHi() {
  return (() => 0)();
}

console.log(typeof sayHi());
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The `sayHi` function returns the returned value of the immediately invoked function (IIFE). This function returned `0`, which is type `"number"`.

FYI: there are only 7 built-in types: `null`, `undefined`, `boolean`, `number`, `string`, `object`, and `symbol`. `"function"` is not a type, since functions are objects, it's of type `"object"`.
</p>
</details>

---

###### 35. Which of these values are falsy?

```javascript
0;
new Number(0);
("");
(" ");
new Boolean(false);
undefined;
```

- A: `0`, `''`, `undefined`
- B: `0`, `new Number(0)`, `''`, `new Boolean(false)`, `undefined`
- C: `0`, `''`, `new Boolean(false)`, `undefined`
- D: All of them are falsy

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

There are only six falsy values:

- `undefined`
- `null`
- `NaN`
- `0`
- `''` (empty string)
- `false`

Function constructors, like `new Number` and `new Boolean` are truthy.

</p>
</details>

---

###### 36. What's the output?

```javascript
console.log(typeof typeof 1);
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`typeof 1` returns `"number"`.
`typeof "number"` returns `"string"`

</p>
</details>

---

###### 37. What's the output?

```javascript
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

When you set a value to an element in an array that exceeds the length of the array, JavaScript creates something called "empty slots". These actually have the value of `undefined`, but you will see something like:

`[1, 2, 3, 7 x empty, 11]`

depending on where you run it (it's different for every browser, node, etc.)

</p>
</details>

---

###### 38. What's the output?

```javascript
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

- A: `1` `undefined` `2`
- B: `undefined` `undefined` `undefined`
- C: `1` `1` `2`
- D: `1` `undefined` `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `catch` block receives the argument `x`. This is not the same `x` as the variable when we pass arguments. This variable `x` is block-scoped.

Later, we set this block-scoped variable equal to `1`, and set the value of the variable `y`. Now, we log the block-scoped variable `x`, which is equal to `1`.

Outside of the `catch` block, `x` is still `undefined`, and `y` is `2`. When we want to `console.log(x)` outside of the `catch` block, it returns `undefined`, and `y` returns `2`.

</p>
</details>

---

###### 39. Everything in JavaScript is either a...

- A: primitive or object
- B: function or object
- C: trick question! only objects
- D: number or object

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

JavaScript only has primitive types and objects.

Primitive types are `boolean`, `null`, `undefined`, `bigint`, `number`, `string`, and `symbol`.

What differentiates a primitive from an object is that primitives do not have any properties or methods; however, you'll note that `'foo'.toUpperCase()` evaluates to `'FOO'` and does not result in a `TypeError`. This is because when you try to access a property or method on a primitive like a string, JavaScript will implicitly wrap the object using one of the wrapper classes, i.e. `String`, and then immediately discard the wrapper after the expression evaluates. All primitives except for `null` and `undefined` exhibit this behaviour.

</p>
</details>

---

###### 40. What's the output?

```javascript
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

- A: `[0, 1, 2, 3, 1, 2]`
- B: `[6, 1, 2]`
- C: `[1, 2, 0, 1, 2, 3]`
- D: `[1, 2, 6]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`[1, 2]` is our initial value. This is the value we start with, and the value of the very first `acc`. During the first round, `acc` is `[1,2]`, and `cur` is `[0, 1]`. We concatenate them, which results in `[1, 2, 0, 1]`.

Then, `[1, 2, 0, 1]` is `acc` and `[2, 3]` is `cur`. We concatenate them, and get `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---

###### 41. What's the output?

```javascript
!!null;
!!"";
!!1;
```

- A: `false` `true` `false`
- B: `false` `false` `true`
- C: `false` `true` `true`
- D: `true` `true` `false`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`null` is falsy. `!null` returns `true`. `!true` returns `false`.

`""` is falsy. `!""` returns `true`. `!true` returns `false`.

`1` is truthy. `!1` returns `false`. `!false` returns `true`.

</p>
</details>

---

###### 42. What does the `setInterval` method return in the browser?

```javascript
setInterval(() => console.log("Hi"), 1000);
```

- A: a unique id
- B: the amount of milliseconds specified
- C: the passed function
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

It returns a unique id. This id can be used to clear that interval with the `clearInterval()` function.

</p>
</details>

---

###### 43. What does this return?

```javascript
[..."Lydia"];
```

- A: `["L", "y", "d", "i", "a"]`
- B: `["Lydia"]`
- C: `[[], "Lydia"]`
- D: `[["L", "y", "d", "i", "a"]]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

A string is an iterable. The spread operator maps every character of an iterable to one element.

</p>
</details>

---

###### 44. What's the output?

```javascript
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);
console.log(gen.next().value);
```

- A: `[0, 10], [10, 20]`
- B: `20, 20`
- C: `10, 20`
- D: `0, 10 and 10, 20`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Regular functions cannot be stopped mid-way after invocation. However, a generator function can be "stopped" midway, and later continue from where it stopped. Every time a generator function encounters a `yield` keyword, the function yields the value specified after it. Note that the generator function in that case doesn’t _return_ the value, it _yields_ the value.

First, we initialize the generator function with `i` equal to `10`. We invoke the generator function using the `next()` method. The first time we invoke the generator function, `i` is equal to `10`. It encounters the first `yield` keyword: it yields the value of `i`. The generator is now "paused", and `10` gets logged.

Then, we invoke the function again with the `next()` method. It starts to continue where it stopped previously, still with `i` equal to `10`. Now, it encounters the next `yield` keyword, and yields `i * 2`. `i` is equal to `10`, so it returns `10 * 2`, which is `20`. This results in `10, 20`.

</p>
</details>

---

###### 45. What does this return?

```javascript
const firstPromise = new Promise((res, rej) => {
  setTimeout(res, 500, "one");
});

const secondPromise = new Promise((res, rej) => {
  setTimeout(res, 100, "two");
});

Promise.race([firstPromise, secondPromise]).then(res => console.log(res));
```

- A: `"one"`
- B: `"two"`
- C: `"two" "one"`
- D: `"one" "two"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

When we pass multiple promises to the `Promise.race` method, it resolves/rejects the _first_ promise that resolves/rejects. To the `setTimeout` method, we pass a timer: 500ms for the first promise (`firstPromise`), and 100ms for the second promise (`secondPromise`). This means that the `secondPromise` resolves first with the value of `'two'`. `res` now holds the value of `'two'`, which gets logged.

</p>
</details>

---

###### 46. What's the output?

```javascript
let person = { name: "Lydia" };
const members = [person];
person = null;

console.log(members);
```

- A: `null`
- B: `[null]`
- C: `[{}]`
- D: `[{ name: "Lydia" }]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

First, we declare a variable `person` with the value of an object that has a `name` property.

<img src="https://i.imgur.com/TML1MbS.png" width="200">

Then, we declare a variable called `members`. We set the first element of that array equal to the value of the `person` variable. Objects interact by _reference_ when setting them equal to each other. When you assign a reference from one variable to another, you make a _copy_ of that reference. (note that they don't have the _same_ reference!)

<img src="https://i.imgur.com/FSG5K3F.png" width="300">

Then, we set the variable `person` equal to `null`.

<img src="https://i.imgur.com/sYjcsMT.png" width="300">

We are only modifying the value of the `person` variable, and not the first element in the array, since that element has a different (copied) reference to the object. The first element in `members` still holds its reference to the original object. When we log the `members` array, the first element still holds the value of the object, which gets logged.

</p>
</details>

---

###### 47. What's the output?

```javascript
const person = {
  name: "Lydia",
  age: 21
};

for (const item in person) {
  console.log(item);
}
```

- A: `{ name: "Lydia" }, { age: 21 }`
- B: `"name", "age"`
- C: `"Lydia", 21`
- D: `["name", "Lydia"], ["age", 21]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

With a `for-in` loop, we can iterate through object keys, in this case `name` and `age`. Under the hood, object keys are strings (if they're not a Symbol). On every loop, we set the value of `item` equal to the current key it’s iterating over. First, `item` is equal to `name`, and gets logged. Then, `item` is equal to `age`, which gets logged.

</p>
</details>

---

###### 48. What's the output?

```javascript
console.log(3 + 4 + "5");
```

- A: `"345"`
- B: `"75"`
- C: `12`
- D: `"12"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Operator associativity is the order in which the compiler evaluates the expressions, either left-to-right or right-to-left. This only happens if all operators have the _same_ precedence. We only have one type of operator: `+`. For addition, the associativity is left-to-right.

`3 + 4` gets evaluated first. This results in the number `7`.

`7 + '5'` results in `"75"` because of coercion. JavaScript converts the number `7` into a string, see question 15. We can concatenate two strings using the `+`operator. `"7" + "5"` results in `"75"`.

</p>
</details>

---

###### 49. What's the value of `num`?

```javascript
const num = parseInt("7*6", 10);
```

- A: `42`
- B: `"42"`
- C: `7`
- D: `NaN`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Only the first numbers in the string is returned. Based on the _radix_ (the second argument in order to specify what type of number we want to parse it to: base 10, hexadecimal, octal, binary, etc.), the `parseInt` checks whether the characters in the string are valid. Once it encounters a character that isn't a valid number in the radix, it stops parsing and ignores the following characters.

`*` is not a valid number. It only parses `"7"` into the decimal `7`. `num` now holds the value of `7`.

</p>
</details>

---

###### 50. What's the output`?

```javascript
[1, 2, 3].map(num => {
  if (typeof num === "number") return;
  return num * 2;
});
```

- A: `[]`
- B: `[null, null, null]`
- C: `[undefined, undefined, undefined]`
- D: `[ 3 x empty ]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

When mapping over the array, the value of `num` is equal to the element it’s currently looping over. In this case, the elements are numbers, so the condition of the if statement `typeof num === "number"` returns `true`. The map function creates a new array and inserts the values returned from the function.

However, we don’t return a value. When we don’t return a value from the function, the function returns `undefined`. For every element in the array, the function block gets called, so for each element we return `undefined`.

</p>
</details>

---

###### 51. What's the output?

```javascript
function getInfo(member, year) {
  member.name = "Lydia";
  year = "1998";
}

const person = { name: "Sarah" };
const birthYear = "1997";

getInfo(person, birthYear);

console.log(person, birthYear);
```

- A: `{ name: "Lydia" }, "1997"`
- B: `{ name: "Sarah" }, "1998"`
- C: `{ name: "Lydia" }, "1998"`
- D: `{ name: "Sarah" }, "1997"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

Arguments are passed by _value_, unless their value is an object, then they're passed by _reference_. `birthYear` is passed by value, since it's a string, not an object. When we pass arguments by value, a _copy_ of that value is created (see question 46).

The variable `birthYear` has a reference to the value `"1997"`. The argument `year` also has a reference to the value `"1997"`, but it's not the same value as `birthYear` has a reference to. When we update the value of `year` by setting `year` equal to `"1998"`, we are only updating the value of `year`. `birthYear` is still equal to `"1997"`.

The value of `person` is an object. The argument `member` has a (copied) reference to the _same_ object. When we modify a property of the object `member` has a reference to, the value of `person` will also be modified, since they both have a reference to the same object. `person`'s `name` property is now equal to the value `"Lydia"`

</p>
</details>

---

###### 52. What's the output?

```javascript
function greeting() {
  throw "Hello world!";
}

function sayHi() {
  try {
    const data = greeting();
    console.log("It worked!", data);
  } catch (e) {
    console.log("Oh no an error:", e);
  }
}

sayHi();
```

- A: `It worked! Hello world!`
- B: `Oh no an error: undefined`
- C: `SyntaxError: can only throw Error objects`
- D: `Oh no an error: Hello world!`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

With the `throw` statement, we can create custom errors. With this statement, you can throw exceptions. An exception can be a <b>string</b>, a <b>number</b>, a <b>boolean</b> or an <b>object</b>. In this case, our exception is the string `'Hello world'`.

With the `catch` statement, we can specify what to do if an exception is thrown in the `try` block. An exception is thrown: the string `'Hello world'`. `e` is now equal to that string, which we log. This results in `'Oh an error: Hello world'`.

</p>
</details>

---

###### 53. What's the output?

```javascript
function Car() {
  this.make = "Lamborghini";
  return { make: "Maserati" };
}

const myCar = new Car();
console.log(myCar.make);
```

- A: `"Lamborghini"`
- B: `"Maserati"`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

When you return a property, the value of the property is equal to the _returned_ value, not the value set in the constructor function. We return the string `"Maserati"`, so `myCar.make` is equal to `"Maserati"`.

</p>
</details>

---

###### 54. What's the output?

```javascript
(() => {
  let x = (y = 10);
})();

console.log(typeof x);
console.log(typeof y);
```

- A: `"undefined", "number"`
- B: `"number", "number"`
- C: `"object", "number"`
- D: `"number", "undefined"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

`let x = y = 10;` is actually shorthand for:

```javascript
y = 10;
let x = y;
```

When we set `y` equal to `10`, we actually add a property `y` to the global object (`window` in browser, `global` in Node). In a browser, `window.y` is now equal to `10`.

Then, we declare a variable `x` with the value of `y`, which is `10`. Variables declared with the `let` keyword are _block scoped_, they are only defined within the block they're declared in; the immediately-invoked function (IIFE) in this case. When we use the `typeof` operator, the operand `x` is not defined: we are trying to access `x` outside of the block it's declared in. This means that `x` is not defined. Values who haven't been assigned a value or declared are of type `"undefined"`. `console.log(typeof x)` returns `"undefined"`.

However, we created a global variable `y` when setting `y` equal to `10`. This value is accessible anywhere in our code. `y` is defined, and holds a value of type `"number"`. `console.log(typeof y)` returns `"number"`.

</p>
</details>

---

###### 55. What's the output?

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
}

Dog.prototype.bark = function() {
  console.log(`Woof I am ${this.name}`);
};

const pet = new Dog("Mara");

pet.bark();

delete Dog.prototype.bark;

pet.bark();
```

- A: `"Woof I am Mara"`, `TypeError`
- B: `"Woof I am Mara"`, `"Woof I am Mara"`
- C: `"Woof I am Mara"`, `undefined`
- D: `TypeError`, `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

We can delete properties from objects using the `delete` keyword, also on the prototype. By deleting a property on the prototype, it is not available anymore in the prototype chain. In this case, the `bark` function is not available anymore on the prototype after `delete Dog.prototype.bark`, yet we still try to access it.

When we try to invoke something that is not a function, a `TypeError` is thrown. In this case `TypeError: pet.bark is not a function`, since `pet.bark` is `undefined`.

</p>
</details>

---

###### 56. What's the output?

```javascript
const set = new Set([1, 1, 2, 3, 4]);

console.log(set);
```

- A: `[1, 1, 2, 3, 4]`
- B: `[1, 2, 3, 4]`
- C: `{1, 1, 2, 3, 4}`
- D: `{1, 2, 3, 4}`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The `Set` object is a collection of _unique_ values: a value can only occur once in a set.

We passed the iterable `[1, 1, 2, 3, 4]` with a duplicate value `1`. Since we cannot have two of the same values in a set, one of them is removed. This results in `{1, 2, 3, 4}`.

</p>
</details>

---

###### 57. What's the output?

```javascript
// counter.js
let counter = 10;
export default counter;
```

```javascript
// index.js
import myCounter from "./counter";

myCounter += 1;

console.log(myCounter);
```

- A: `10`
- B: `11`
- C: `Error`
- D: `NaN`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

An imported module is _read-only_: you cannot modify the imported module. Only the module that exports them can change its value.

When we try to increment the value of `myCounter`, it throws an error: `myCounter` is read-only and cannot be modified.

</p>
</details>

---

###### 58. What's the output?

```javascript
const name = "Lydia";
age = 21;

console.log(delete name);
console.log(delete age);
```

- A: `false`, `true`
- B: `"Lydia"`, `21`
- C: `true`, `true`
- D: `undefined`, `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `delete` operator returns a boolean value: `true` on a successful deletion, else it'll return `false`. However, variables declared with the `var`, `const` or `let` keyword cannot be deleted using the `delete` operator.

The `name` variable was declared with a `const` keyword, so its deletion is not successful: `false` is returned. When we set `age` equal to `21`, we actually added a property called `age` to the global object. You can successfully delete properties from objects this way, also the global object, so `delete age` returns `true`.

</p>
</details>

---

###### 59. What's the output?

```javascript
const numbers = [1, 2, 3, 4, 5];
const [y] = numbers;

console.log(y);
```

- A: `[[1, 2, 3, 4, 5]]`
- B: `[1, 2, 3, 4, 5]`
- C: `1`
- D: `[1]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

We can unpack values from arrays or properties from objects through destructuring. For example:

```javascript
[a, b] = [1, 2];
```

<img src="https://i.imgur.com/ADFpVop.png" width="200">

The value of `a` is now `1`, and the value of `b` is now `2`. What we actually did in the question, is:

```javascript
[y] = [1, 2, 3, 4, 5];
```

<img src="https://i.imgur.com/NzGkMNk.png" width="200">

This means that the value of `y` is equal to the first value in the array, which is the number `1`. When we log `y`, `1` is returned.

</p>
</details>

---

###### 60. What's the output?

```javascript
const user = { name: "Lydia", age: 21 };
const admin = { admin: true, ...user };

console.log(admin);
```

- A: `{ admin: true, user: { name: "Lydia", age: 21 } }`
- B: `{ admin: true, name: "Lydia", age: 21 }`
- C: `{ admin: true, user: ["Lydia", 21] }`
- D: `{ admin: true }`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

It's possible to combine objects using the spread operator `...`. It lets you create copies of the key/value pairs of one object, and add them to another object. In this case, we create copies of the `user` object, and add them to the `admin` object. The `admin` object now contains the copied key/value pairs, which results in `{ admin: true, name: "Lydia", age: 21 }`.

</p>
</details>

---

###### 61. What's the output?

```javascript
const person = { name: "Lydia" };

Object.defineProperty(person, "age", { value: 21 });

console.log(person);
console.log(Object.keys(person));
```

- A: `{ name: "Lydia", age: 21 }`, `["name", "age"]`
- B: `{ name: "Lydia", age: 21 }`, `["name"]`
- C: `{ name: "Lydia"}`, `["name", "age"]`
- D: `{ name: "Lydia"}`, `["age"]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

With the `defineProperty` method, we can add new properties to an object, or modify existing ones. When we add a property to an object using the `defineProperty` method, they are by default _not enumerable_. The `Object.keys` method returns all _enumerable_ property names from an object, in this case only `"name"`.

Properties added using the `defineProperty` method are immutable by default. You can override this behavior using the `writable`, `configurable` and `enumerable` properties. This way, the `defineProperty` method gives you a lot more control over the properties you're adding to an object.

</p>
</details>

---

###### 62. What's the output?

```javascript
const settings = {
  username: "lydiahallie",
  level: 19,
  health: 90
};

const data = JSON.stringify(settings, ["level", "health"]);
console.log(data);
```

- A: `"{"level":19, "health":90}"`
- B: `"{"username": "lydiahallie"}"`
- C: `"["level", "health"]"`
- D: `"{"username": "lydiahallie", "level":19, "health":90}"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The second argument of `JSON.stringify` is the _replacer_. The replacer can either be a function or an array, and lets you control what and how the values should be stringified.

If the replacer is an _array_, only the property names included in the array will be added to the JSON string. In this case, only the properties with the names `"level"` and `"health"` are included, `"username"` is excluded. `data` is now equal to `"{"level":19, "health":90}"`.

If the replacer is a _function_, this function gets called on every property in the object you're stringifying. The value returned from this function will be the value of the property when it's added to the JSON string. If the value is `undefined`, this property is excluded from the JSON string.

</p>
</details>

---

###### 63. What's the output?

```javascript
let num = 10;

const increaseNumber = () => num++;
const increasePassedNumber = number => number++;

const num1 = increaseNumber();
const num2 = increasePassedNumber(num1);

console.log(num1);
console.log(num2);
```

- A: `10`, `10`
- B: `10`, `11`
- C: `11`, `11`
- D: `11`, `12`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The unary operator `++` _first returns_ the value of the operand, _then increments_ the value of the operand. The value of `num1` is `10`, since the `increaseNumber` function first returns the value of `num`, which is `10`, and only increments the value of `num` afterwards.

`num2` is `10`, since we passed `num1` to the `increasePassedNumber`. `number` is equal to `10`(the value of `num1`. Again, the unary operator `++` _first returns_ the value of the operand, _then increments_ the value of the operand. The value of `number` is `10`, so `num2` is equal to `10`.

</p>
</details>

---

###### 64. What's the output?

```javascript
const value = { number: 10 };

const multiply = (x = { ...value }) => {
  console.log((x.number *= 2));
};

multiply();
multiply();
multiply(value);
multiply(value);
```

- A: `20`, `40`, `80`, `160`
- B: `20`, `40`, `20`, `40`
- C: `20`, `20`, `20`, `40`
- D: `NaN`, `NaN`, `20`, `40`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

In ES6, we can initialize parameters with a default value. The value of the parameter will be the default value, if no other value has been passed to the function, or if the value of the parameter is `"undefined"`. In this case, we spread the properties of the `value` object into a new object, so `x` has the default value of `{ number: 10 }`.

The default argument is evaluated at _call time_! Every time we call the function, a _new_ object is created. We invoke the `multiply` function the first two times without passing a value: `x` has the default value of `{ number: 10 }`. We then log the multiplied value of that number, which is `20`.

The third time we invoke multiply, we do pass an argument: the object called `value`. The `*=` operator is actually shorthand for `x.number = x.number * 2`: we modify the value of `x.number`, and log the multiplied value `20`. 

The fourth time, we pass the `value` object again. `x.number` was previously modified to `20`, so `x.number *= 2` logs `40`. 

</p>
</details>

---

###### 65. What's the output?

```javascript
[1, 2, 3, 4].reduce((x, y) => console.log(x, y));
```

- A: `1` `2` and `3` `3` and `6` `4`
- B: `1` `2` and `2` `3` and `3` `4`
- C: `1` `undefined` and `2` `undefined` and `3` `undefined` and `4` `undefined`
- D: `1` `2` and `undefined` `3` and `undefined` `4`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The first argument that the `reduce` method receives is the _accumulator_, `x` in this case. The second argument is the _current value_, `y`. With the reduce method, we execute a callback function on every element in the array, which could ultimately result in one single value. 

In this example, we are not returning any values, we are simply logging the values of the accumulator and the current value.

The value of the accumulator is equal to the previously returned value of the callback function. If you don't pass the optional `initialValue` argument to the `reduce` method, the accumulator is equal to the first element on the first call.

On the first call, the accumulator (`x`) is `1`, and the current value (`y`) is `2`. We don't return from the callback function, we log the accumulator and current value: `1` and `2` get logged.  

If you don't return a value from a function, it returns `undefined`. On the next call, the accumulator is `undefined`, and the current value is `3`. `undefined` and `3` get logged. 

On the fourth call, we again don't return from the callback function. The accumulator is again `undefined`, and the current value is `4`. `undefined` and `4` get logged.
</p>
</details>
  
---

###### 66. With which constructor can we successfully extend the `Dog` class?

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
};

class Labrador extends Dog {
  // 1 
  constructor(name, size) {
    this.size = size;
  }
  // 2
  constructor(name, size) {
    super(name);
    this.size = size;
  }
  // 3
  constructor(size) {
    super(name);
    this.size = size;
  }
  // 4 
  constructor(name, size) {
    this.name = name;
    this.size = size;
  }

};
```

- A: 1
- B: 2
- C: 3
- D: 4

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

In a derived class, you cannot access the `this` keyword before calling `super`. If you try to do that, it will throw a ReferenceError: 1 and 4 would throw a reference error.

With the `super` keyword, we call that parent class's constructor with the given arguments. The parent's constructor receives the `name` argument, so we need to pass `name` to `super`. 

The `Labrador` class receives two arguments, `name` since it extends `Dog`, and `size` as an extra property on the `Labrador` class. They both need to be passed to the constructor function on `Labrador`, which is done correctly  using constructor 2.
</p>
</details>

---

###### 67. What's the output?

```javascript
// index.js
console.log('running index.js');
import { sum } from './sum.js';
console.log(sum(1, 2));

// sum.js
console.log('running sum.js');
export const sum = (a, b) => a + b;
```

- A: `running index.js`, `running sum.js`, `3`
- B: `running sum.js`, `running index.js`, `3`
- C: `running sum.js`, `3`, `running index.js`
- D: `running index.js`, `undefined`, `running sum.js`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

With the `import` keyword, all imported modules are _pre-parsed_. This means that the imported modules get run _first_, the code in the file which imports the module gets executed _after_.

This is a difference between `require()` in CommonJS and `import`! With `require()`, you can load dependencies on demand while the code is being run. If we would have used `require` instead of `import`, `running index.js`, `running sum.js`, `3` would have been logged to the console. 

</p>
</details>

---

###### 68. What's the output?

```javascript
console.log(Number(2) === Number(2))
console.log(Boolean(false) === Boolean(false))
console.log(Symbol('foo') === Symbol('foo'))
```

- A: `true`, `true`, `false`
- B: `false`, `true`, `false`
- C: `true`, `false`, `true`
- D: `true`, `true`, `true`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

Every Symbol is entirely unique. The purpose of the argument passed to the Symbol is to give the Symbol a description. The value of the Symbol is not dependent on the passed argument. As we test equality, we are creating two entirely new symbols: the first `Symbol('foo')`, and the second `Symbol('foo')`. These two values are unique and not equal to each other, `Symbol('foo') === Symbol('foo')` returns `false`. 

</p>
</details>

---

###### 69. What's the output?

```javascript
const name = "Lydia Hallie"
console.log(name.padStart(13))
console.log(name.padStart(2))
```

- A: `"Lydia Hallie"`, `"Lydia Hallie"`
- B: `"           Lydia Hallie"`, `"  Lydia Hallie"` (`"[13x whitespace]Lydia Hallie"`, `"[2x whitespace]Lydia Hallie"`)
- C: `" Lydia Hallie"`, `"Lydia Hallie"` (`"[1x whitespace]Lydia Hallie"`, `"Lydia Hallie"`)
- D: `"Lydia Hallie"`, `"Lyd"`, 

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

With the `padStart` method, we can add padding to the beginning of a string. The value passed to this method is the _total_ length of the string together with the padding. The string `"Lydia Hallie"` has a length of `12`. `name.padStart(13)` inserts 1 space at the start of the string, because 12 + 1 is 13.

If the argument passed to the `padStart` method is smaller than the length of the array, no padding will be added.

</p>
</details>

---

###### 70. What's the output?

```javascript
console.log("🥑" + "💻");
```

- A: `"🥑💻"`
- B: `257548`
- C: A string containing their code points
- D: Error

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

With the `+` operator, you can concatenate strings. In this case, we are concatenating the string `"🥑"` with the string `"💻"`, resulting in `"🥑💻"`.

</p>
</details>

---

###### 71. How can we log the values that are commented out after the console.log statement?

```javascript
function* startGame() {
  const answer = yield "Do you love JavaScript?";
  if (answer !== "Yes") {
    return "Oh wow... Guess we're gone here";
  }
  return "JavaScript loves you back ❤️";
}

const game = startGame();
console.log(/* 1 */); // Do you love JavaScript?
console.log(/* 2 */); // JavaScript loves you back ❤️
```

- A: `game.next("Yes").value` and `game.next().value`
- B: `game.next.value("Yes")` and `game.next.value()`
- C: `game.next().value` and `game.next("Yes").value`
- D: `game.next.value()` and `game.next.value("Yes")`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

A generator function "pauses" its execution when it sees the `yield` keyword. First, we have to let the function yield the string "Do you love JavaScript?", which can be done by calling `game.next().value`.

Every line is executed, until it finds the first `yield` keyword. There is a `yield` keyword on the first line within the function: the execution stops with the first yield! _This means that the variable `answer` is not defined yet!_

When we call `game.next("Yes").value`, the previous `yield` is replaced with the value of the parameters passed to the `next()` function, `"Yes"` in this case. The value of the variable `answer` is now equal to `"Yes"`. The condition of the if-statement returns `false`, and `JavaScript loves you back ❤️` gets logged.

</p>
</details>

---

###### 72. What's the output?

```javascript
console.log(String.raw`Hello\nworld`);
```

- A: `Hello world!`
- B: `Hello` <br />&nbsp; &nbsp; &nbsp;`world`
- C: `Hello\nworld`
- D: `Hello\n` <br /> &nbsp; &nbsp; &nbsp;`world`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`String.raw` returns a string where the escapes (`\n`, `\v`, `\t` etc.) are ignored! Backslashes can be an issue since you could end up with something like:

`` const path = `C:\Documents\Projects\table.html` ``

Which would result in:

`"C:DocumentsProjects able.html"`

With `String.raw`, it would simply ignore the escape and print:

`C:\Documents\Projects\table.html`

In this case, the string is `Hello\nworld`, which gets logged.

</p>
</details>

---

###### 73. What's the output?

```javascript
async function getData() {
  return await Promise.resolve("I made it!");
}

const data = getData();
console.log(data);
```

- A: `"I made it!"`
- B: `Promise {<resolved>: "I made it!"}`
- C: `Promise {<pending>}`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

An async function always returns a promise. The `await` still has to wait for the promise to resolve: a pending promise gets returned when we call `getData()` in order to set `data` equal to it.

If we wanted to get access to the resolved value `"I made it"`, we could have used the `.then()` method on `data`:

`data.then(res => console.log(res))`

This would've logged `"I made it!"`

</p>
</details>

---

###### 74. What's the output?

```javascript
function addToList(item, list) {
  return list.push(item);
}

const result = addToList("apple", ["banana"]);
console.log(result);
```

- A: `['apple', 'banana']`
- B: `2`
- C: `true`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The `.push()` method returns the _length_ of the new array! Previously, the array contained one element (the string `"banana"`) and had a length of `1`. After adding the string `"apple"` to the array, the array contains two elements, and has a length of `2`. This gets returned from the `addToList` function.

The `push` method modifies the original array. If you wanted to return the _array_ from the function rather than the _length of the array_, you should have returned `list` after pushing `item` to it.

</p>
</details>

---

###### 75. What's the output?

```javascript
const box = { x: 10, y: 20 };

Object.freeze(box);

const shape = box;
shape.x = 100;

console.log(shape);
```

- A: `{ x: 100, y: 20 }`
- B: `{ x: 10, y: 20 }`
- C: `{ x: 100 }`
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`Object.freeze` makes it impossible to add, remove, or modify properties of an object (unless the property's value is another object).

When we create the variable `shape` and set it equal to the frozen object `box`, `shape` also refers to a frozen object. You can check whether an object is frozen by using `Object.isFrozen`. In this case, `Object.isFrozen(shape)` returns true, since the variable `shape` has a reference to a frozen object.

Since `shape` is frozen, and since the value of `x` is not an object, we cannot modify the property `x`. `x` is still equal to `10`, and `{ x: 10, y: 20 }` gets logged.

</p>
</details>

---

###### 76. What's the output?

```javascript
const { name: myName } = { name: "Lydia" };

console.log(name);
```

- A: `"Lydia"`
- B: `"myName"`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

When we unpack the property `name` from the object on the right-hand side, we assign its value `"Lydia"` to a variable with the name `myName`.

With `{ name: myName }`, we tell JavaScript that we want to create a new variable called `myName` with the value of the `name` property on the right-hand side.

Since we try to log `name`, a variable that is not defined, a ReferenceError gets thrown.

</p>
</details>

---

###### 77. Is this a pure function?

```javascript
function sum(a, b) {
  return a + b;
}
```

- A: Yes
- B: No

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

A pure function is a function that _always_ returns the same result, if the same arguments are passed.

The `sum` function always returns the same result. If we pass `1` and `2`, it will _always_ return `3` without side effects. If we pass `5` and `10`, it will _always_ return `15`, and so on. This is the definition of a pure function.

</p>
</details>

---

###### 78. What is the output?

```javascript
const add = () => {
  const cache = {};
  return num => {
    if (num in cache) {
      return `From cache! ${cache[num]}`;
    } else {
      const result = num + 10;
      cache[num] = result;
      return `Calculated! ${result}`;
    }
  };
};

const addFunction = add();
console.log(addFunction(10));
console.log(addFunction(10));
console.log(addFunction(5 * 2));
```

- A: `Calculated! 20` `Calculated! 20` `Calculated! 20`
- B: `Calculated! 20` `From cache! 20` `Calculated! 20`
- C: `Calculated! 20` `From cache! 20` `From cache! 20`
- D: `Calculated! 20` `From cache! 20` `Error`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The `add` function is a _memoized_ function. With memoization, we can cache the results of a function in order to speed up its execution. In this case, we create a `cache` object that stores the previously returned values.

If we call the `addFunction` function again with the same argument, it first checks whether it has already gotten that value in its cache. If that's the case, the caches value will be returned, which saves on execution time. Else, if it's not cached, it will calculate the value and store it afterwards.

We call the `addFunction` function three times with the same value: on the first invocation, the value of the function when `num` is equal to `10` isn't cached yet. The condition of the if-statement `num in cache` returns `false`, and the else block gets executed: `Calculated! 20` gets logged, and the value of the result gets added to the cache object. `cache` now looks like `{ 10: 20 }`.

The second time, the `cache` object contains the value that gets returned for `10`. The condition of the if-statement `num in cache` returns `true`, and `'From cache! 20'` gets logged.

The third time, we pass `5 * 2` to the function which gets evaluated to `10`. The `cache` object contains the value that gets returned for `10`. The condition of the if-statement `num in cache` returns `true`, and `'From cache! 20'` gets logged.

</p>
</details>

---

###### 79. What is the output?

```javascript
const myLifeSummedUp = ["☕", "💻", "🍷", "🍫"]

for (let item in myLifeSummedUp) {
  console.log(item)
}

for (let item of myLifeSummedUp) {
  console.log(item)
}
```

- A: `0` `1` `2` `3` and `"☕"` ` "💻"` `"🍷"` `"🍫"`
- B: `"☕"` ` "💻"` `"🍷"` `"🍫"` and `"☕"` ` "💻"` `"🍷"` `"🍫"`
- C: `"☕"` ` "💻"` `"🍷"` `"🍫"` and `0` `1` `2` `3`
- D:  `0` `1` `2` `3` and `{0: "☕", 1: "💻", 2: "🍷", 3: "🍫"}`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

With a _for-in_ loop, we can iterate over **enumerable** properties. In an array, the enumerable properties are the "keys" of array elements, which are actually their indexes. You could see an array as:

`{0: "☕", 1: "💻", 2: "🍷", 3: "🍫"}`

Where the keys are the enumerable properties. `0` `1` `2` `3` get logged.

With a _for-of_ loop, we can iterate over **iterables**. An array is an iterable. When we iterate over the array, the variable "item" is equal to the element it's currently iterating over, `"☕"` ` "💻"` `"🍷"` `"🍫"` get logged.

</p>
</details>

---

###### 80. What is the output?

```javascript
const list = [1 + 2, 1 * 2, 1 / 2]
console.log(list)
```

- A: `["1 + 2", "1 * 2", "1 / 2"]`
- B: `["12", 2, 0.5]`
- C: `[3, 2, 0.5]`
- D:  `[1, 1, 1]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Array elements can hold any value. Numbers, strings, objects, other arrays, null, boolean values, undefined, and other expressions such as dates, functions, and calculations.

The element will be equal to the returned value.  `1 + 2` returns `3`, `1 * 2` returns `2`, and `1 / 2` returns `0.5`.

</p>
</details>

---

###### 81. What is the output?

```javascript
function sayHi(name) {
  return `Hi there, ${name}`
}

console.log(sayHi())
```

- A: `Hi there, `
- B: `Hi there, undefined`
- C: `Hi there, null`
- D:  `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

By default, arguments have the value of `undefined`, unless a value has been passed to the function. In this case, we didn't pass a value for the `name` argument. `name` is equal to `undefined` which gets logged.

In ES6, we can overwrite this default `undefined` value with default parameters. For example:

`function sayHi(name = "Lydia") { ... }`

In this case, if we didn't pass a value or if we passed `undefined`, `name` would always be equal to the string `Lydia`

</p>
</details>

---

###### 82. What is the output?

```javascript
var status = "😎"

setTimeout(() => {
  const status = "😍"

  const data = {
    status: "🥑",
    getStatus() {
      return this.status
    }
  }

  console.log(data.getStatus())
  console.log(data.getStatus.call(this))
}, 0)
```

- A: `"🥑"` and `"😍"`
- B: `"🥑"` and `"😎"`
- C: `"😍"` and `"😎"`
- D: `"😎"` and `"😎"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The value of the `this` keyword is dependent on where you use it. In a **method**, like the `getStatus` method, the `this` keyword refers to _the object that the method belongs to_. The method belongs to the `data` object, so `this` refers to the `data` object. When we log `this.status`, the `status` property on the `data` object gets logged, which is `"🥑"`.

With the `call` method, we can change the object to which the `this` keyword refers. In **functions**, the `this` keyword refers to the _the object that the function belongs to_. We declared the `setTimeout` function on the _global object_, so within the `setTimeout` function, the `this` keyword refers to the _global object_. On the global object, there is a variable called _status_ with the value of `"😎"`. When logging `this.status`, `"😎"` gets logged.


</p>
</details>

---

###### 83. What is the output?

```javascript
const person = {
  name: "Lydia",
  age: 21
}

let city = person.city
city = "Amsterdam"

console.log(person)
```

- A: `{ name: "Lydia", age: 21 }`
- B: `{ name: "Lydia", age: 21, city: "Amsterdam" }`
- C: `{ name: "Lydia", age: 21, city: undefined }`
- D: `"Amsterdam"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

We set the variable `city` equal to the value of the property called `city` on the `person` object. There is no property on this object called `city`, so the variable `city` has the value of `undefined`. 

Note that we are _not_ referencing the `person` object itself! We simply set the variable `city` equal to the current value of the `city` property on the `person` object.

Then, we set `city` equal to the string `"Amsterdam"`. This doesn't change the person object: there is no reference to that object.

When logging the `person` object, the unmodified object gets returned. 

</p>
</details>

---

###### 84. What is the output?

```javascript
function checkAge(age) {
  if (age < 18) {
    const message = "Sorry, you're too young."
  } else {
    const message = "Yay! You're old enough!"
  }

  return message
}

console.log(checkAge(21))
```

- A: `"Sorry, you're too young."`
- B: `"Yay! You're old enough!"`
- C: `ReferenceError`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Variables with the `const` and `let` keyword are _block-scoped_. A block is anything between curly brackets (`{ }`). In this case, the curly brackets of the if/else statements. You cannot reference a variable outside of the block it's declared in, a ReferenceError gets thrown.

</p>
</details>

---

###### 85. What kind of information would get logged?

```javascript
fetch('https://www.website.com/api/user/1')
  .then(res => res.json())
  .then(res => console.log(res))
```

- A: The result of the `fetch` method.
- B: The result of the second invocation of the `fetch` method.
- C: The result of the callback in the previous `.then()`.
- D: It would always be undefined. 

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The value of `res` in the second `.then` is equal to the returned value of the previous `.then`. You can keep chaining `.then`s like this, where the value is passed to the next handler.

</p>
</details>

---

###### 86. Which option is a way to set `hasName` equal to `true`, provided you cannot pass `true` as an argument?

```javascript
function getName(name) {
  const hasName = //
}
```

- A: `!!name`
- B: `name`
- C: `new Boolean(name)`
- D: `name.length`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

With `!!name`, we determine whether the value of `name` is truthy or falsy. If name is truthy, which we want to test for, `!name` returns `false`. `!false` (which is what `!!name` practically is) returns `true`.

By setting `hasName` equal to `name`, you set `hasName` equal to whatever value you passed to the `getName` function, not the boolean value `true`.

`new Boolean(true)` returns an object wrapper, not the boolean value itself.

`name.length` returns the length of the passed argument, not whether it's `true`.

</p>
</details>

---

###### 87. What's the output?

```javascript
console.log("I want pizza"[0])
```

- A: `"""`
- B: `"I"`
- C: `SyntaxError`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

In order to get an character on a specific index in a string, you can use bracket notation. The first character in the string has index 0, and so on. In this case we want to get the element which index is 0, the character `"I'`, which gets logged.

Note that this method is not supported in IE7 and below. In that case, use `.charAt()`

</p>
</details>

---

###### 88. What's the output?

```javascript
function sum(num1, num2 = num1) {
  console.log(num1 + num2)
}

sum(10)
```

- A: `NaN`
- B: `20`
- C: `ReferenceError`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

You can set a default parameter's value equal to another parameter of the function, as long as they've been defined _before_ the default parameter. We pass the value `10` to the `sum` function. If the `sum` function only receives 1 argument, it means that the value for `num2` is not passed, and the value of `num1` is equal to the passed value `10` in this case. The default value of `num2` is the value of `num1`, which is `10`.  `num1 + num2` returns `20`.

If you're trying to set a default parameter's value equal to a parameter which is defined _after_ (to the right), the parameter's value hasn't been initialized yet, which will throw an error. 

</p>
</details>

---

###### 89. What's the output?

```javascript
// module.js 
export default () => "Hello world"
export const name = "Lydia"

// index.js 
import * as data from "./module"

console.log(data)
```

- A: `{ default: function default(), name: "Lydia" }`
- B: `{ default: function default() }`
- C: `{ default: "Hello world", name: "Lydia" }`
- D: Global object of `module.js`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

With the `import * as name` syntax, we import _all exports_ from the `module.js` file into the `index.js` file as a new object called `data` is created. In the `module.js` file, there are two exports: the default export, and a named export. The default export is a function which returns the string `"Hello World"`, and the named export is a variable called `name` which has the value of the string `"Lydia"`. 

The `data` object has a `default` property for the default export, other properties have the names of the named exports and their corresponding values. 

</p>
</details>

---

###### 90. What's the output?

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
}

const member = new Person("John")
console.log(typeof member)
```

- A: `"class"`
- B: `"function"`
- C: `"object"`
- D: `"string"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Classes are syntactical sugar for function constructors. The equivalent of the `Person` class as a function constructor would be:

```javascript
function Person() {
  this.name = name
}
```

Calling a function constructor with `new` results in the creation of an instance of `Person`, `typeof` keyword returns `"object"` for an instance. `typeof member` returns `"object"`. 

</p>
</details>

---

###### 91. What's the output?

```javascript
let newList = [1, 2, 3].push(4)

console.log(newList.push(5))
```

- A: `[1, 2, 3, 4, 5]`
- B: `[1, 2, 3, 5]`
- C: `[1, 2, 3, 4]`
- D: `Error`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The `.push` method returns the _new length_ of the array, not the array itself! By setting `newList` equal to `[1, 2, 3].push(4)`, we set `newList` equal to the new length of the array: `4`. 

Then, we try to use the `.push` method on `newList`. Since `newList` is the numerical value `4`, we cannot use the `.push` method: a TypeError is thrown.

</p>
</details>

---

###### 92. What's the output?

```javascript
function giveLydiaPizza() {
  return "Here is pizza!"
}

const giveLydiaChocolate = () => "Here's chocolate... now go hit the gym already."

console.log(giveLydiaPizza.prototype)
console.log(giveLydiaChocolate.prototype)
```

- A: `{ constructor: ...}` `{ constructor: ...}` 
- B: `{}` `{ constructor: ...}` 
- C: `{ constructor: ...}` `{}`
- D: `{ constructor: ...}` `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

Regular functions, such as the `giveLydiaPizza` function, have a `prototype` property, which is an object (prototype object) with a `constructor` property. Arrow functions however, such as the `giveLydiaChocolate` function, do not have this `prototype` property. `undefined` gets returned when trying to access the `prototype` property using `giveLydiaChocolate.prototype`. 

</p>
</details>

---

###### 93. What's the output?

```javascript
const person = {
  name: "Lydia",
  age: 21
}

for (const [x, y] of Object.entries(person)) {
  console.log(x, y)
}
```

- A: `name` `Lydia` and `age` `21`
- B: `["name", "Lydia"]` and `["age", 21]` 
- C: `["name", "age"]` and `undefined`
- D: `Error`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

`Object.entries(person)` returns an array of nested arrays, containing the keys and objects:

`[ [ 'name', 'Lydia' ], [ 'age', 21 ] ]` 

Using the `for-of` loop, we can iterate over each element in the array, the subarrays in this case. We can destructure the subarrays instantly in the for-of loop, using `const [x, y]`. `x` is equal to the first element in the subarray, `y` is equal to the second element in the subarray. 

The first subarray is `[ "name", "Lydia" ]`, with `x` equal to `"name"`, and `y` equal to `"Lydia"`, which get logged.
The second subarray is `[ "age", 21 ]`, with `x` equal to `"age"`, and `y` equal to `21`, which get logged.

</p>
</details>

---

###### 94. What's the output?

```javascript
function getItems(fruitList, ...args, favoriteFruit) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```

- A: `["banana", "apple", "pear", "orange"]`
- B: `[["banana", "apple"], "pear", "orange"]` 
- C: `["banana", "apple", ["pear"], "orange"]`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

`...args` is a rest parameter. The rest parameter's value is an array containing all remaining arguments, **and can only be the last parameter**! In this example, the rest parameter was the second parameter. This is not possible, and will throw a syntax error. 

```javascript
function getItems(fruitList, favoriteFruit, ...args) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```

The above example works. This returns the array `[ 'banana', 'apple', 'orange', 'pear' ]`
</p>
</details>

---

###### 95. What's the output?

```javascript
function nums(a, b) {
  if
  (a > b)
  console.log('a is bigger')
  else 
  console.log('b is bigger')
  return 
  a + b
}

console.log(nums(4, 2))
console.log(nums(1, 2))
```

- A: `a is bigger`, `6` and `b is bigger`, `3`
- B: `a is bigger`, `undefined` and `b is bigger`, `undefined`
- C: `undefined` and `undefined`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

In JavaScript, we don't _have_ to write the semicolon (`;`) explicitly, however the JavaScript engine still adds them after statements. This is called **Automatic Semicolon Insertion**. A statement can for example be variables, or keywords like `throw`, `return`, `break`, etc. 

Here, we wrote a `return` statement, and another value `a + b` on a _new line_. However, since it's a new line, the engine doesn't know that it's actually the value that we wanted to return. Instead, it automatically added a semicolon after `return`. You could see this as:

```javascript
  return;
  a + b
```

This means that `a + b` is never reached, since a function stops running after the `return` keyword. If no value gets returned, like here, the function returns `undefined`. Note that there is no automatic insertion after `if/else` statements!

</p>
</details>

---

###### 96. What's the output?

```javascript
class Person {
  constructor() {
    this.name = "Lydia"
  }
}

Person = class AnotherPerson {
  constructor() {
    this.name = "Sarah"
  }
}

const member = new Person()
console.log(member.name)
```

- A: `"Lydia"`
- B: `"Sarah"`
- C: `Error: cannot redeclare Person`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

We can set classes equal to other classes/function constructors. In this case, we set `Person` equal to `AnotherPerson`. The name on this constructor is `Sarah`, so the name property on the new `Person` instance `member` is `"Sarah"`.

</p>
</details>

---

###### 97. What's the output?

```javascript
const info = {
  [Symbol('a')]: 'b'
}

console.log(info)
console.log(Object.keys(info))
```

- A: `{Symbol('a'): 'b'}` and `["{Symbol('a')"]`
- B: `{}` and `[]`
- C: `{ a: "b" }` and `["a"]`
- D: `{Symbol('a'): 'b'}` and `[]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

A Symbol is not _enumerable_. The Object.keys method returns all _enumerable_ key properties on an object. The Symbol won't be visible, and an empty array is returned. When logging the entire object, all properties will be visible, even non-enumerable ones.

This is one of the many qualities of a symbol: besides representing an entirely unique value (which prevents accidental name collision on objects, for example when working with 2 libraries that want to add properties to the same object), you can also "hide" properties on objects this way (although not entirely. You can still access symbols using the `Object.getOwnPropertySymbols()` method).

</p>
</details>

---

###### 98. What's the output?

```javascript
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age }

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list))
console.log(getUser(user))
```

- A: `[1, [2, 3, 4]]` and `undefined`
- B: `[1, [2, 3, 4]]` and `{ name: "Lydia", age: 21 }`
- C: `[1, 2, 3, 4]` and `{ name: "Lydia", age: 21 }`
- D: `Error` and `{ name: "Lydia", age: 21 }`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `getList` function receives an array as its argument. Between the parentheses of the `getList` function, we destructure this array right away. You could see this as:

 `[x, ...y] = [1, 2, 3, 4]`

 With the rest parameter `...y`, we put all "remaining" arguments in an array. The remaining arguments are `2`, `3` and `4` in this case. The value of `y` is an array, containing all the rest parameters. The value of `x` is equal to `1` in this case, so when we log `[x, y]`, `[1, [2, 3, 4]]` gets logged.

 The `getUser` function receives an object. With arrow functions, we don't _have_ to write curly brackets if we just return one value. However, if you want to return an _object_ from an arrow function, you have to write it between parentheses, otherwise no value gets returned! The following function would have returned an object:

```const getUser = user => ({ name: user.name, age: user.age })```

Since no value gets returned in this case, the function returns `undefined`.

</p>
</details>

---

###### 99. What's the output?

```javascript
const name = "Lydia"

console.log(name())
```

- A: `SyntaxError`
- B: `ReferenceError`
- C: `TypeError`
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The variable `name` holds the value of a string, which is not a function, thus cannot invoke. 

TypeErrors get thrown when a value is not of the expected type. JavaScript expected `name` to be a function since we're trying to invoke it. It was a string however, so a TypeError gets thrown: name is not a function!

SyntaxErrors get thrown when you've written something that isn't valid JavaScript, for example when you've written the word `return` as `retrun`. 
ReferenceErrors get thrown when JavaScript isn't able to find a reference to a value that you're trying to access.

</p>
</details>

---

###### 100. What's the value of output?

```javascript
// 🎉✨ This is my 100th question! ✨🎉

const output = `${[] && 'Im'}possible!
You should${'' && `n't`} see a therapist after so much JavaScript lol`
```

- A: `possible! You should see a therapist after so much JavaScript lol`
- B: `Impossible! You should see a therapist after so much JavaScript lol`
- C: `possible! You shouldn't see a therapist after so much JavaScript lol`
- D: `Impossible! You shouldn't see a therapist after so much JavaScript lol`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`[]` is a truthy value. With the `&&` operator, the right-hand value will be returned if the left-hand value is a truthy value. In this case, the left-hand value `[]` is a truthy value, so `"Im'` gets returned.

`""` is a falsy value. If the left-hand value is falsy, nothing gets returned. `n't` doesn't get returned.

</p>
</details>

---

###### 101. What's the value of output?

```javascript
const one = (false || {} || null)
const two = (null || false || "")
const three = ([] || 0 || true)

console.log(one, two, three)
```

- A: `false` `null` `[]`
- B: `null` `""` `true`
- C: `{}` `""` `[]`
- D: `null` `null` `true`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

With the `||` operator, we can return the first truthy operand. If all values are falsy, the last operand gets returned.

`(false || {} || null)`: the empty object `{}` is a truthy value. This is the first (and only) truthy value, which gets returned. `one` is equal to `{}`.

`(null || false || "")`: all operands are falsy values. This means that the past operand, `""` gets returned. `two` is equal to `""`.

`([] || 0 || "")`: the empty array`[]` is a truthy value. This is the first truthy value, which gets returned. `three` is equal to `[]`.

</p>
</details>

---

###### 102. What's the value of output?

```javascript
const myPromise = () => Promise.resolve('I have resolved!')

function firstFunction() {
  myPromise().then(res => console.log(res))
  console.log('second')
}

async function secondFunction() {
  console.log(await myPromise())
  console.log('second')
}

firstFunction()
secondFunction()
```

- A: `I have resolved!`, `second` and `I have resolved!`, `second`
- B: `second`, `I have resolved!` and `second`, `I have resolved!`
- C: `I have resolved!`, `second` and `second`, `I have resolved!`
- D: `second`, `I have resolved!` and `I have resolved!`, `second`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

With a promise, we basically say _I want to execute this function, but I'll put it aside for now while it's running since this might take a while. Only when a certain value is resolved (or rejected), and when the call stack is empty, I want to use this value._

We can get this value with both `.then` and the `await` keyword in an `async` function. Although we can get a promise's value with both `.then` and `await`, they work a bit differently. 

In the `firstFunction`, we (sort of) put the myPromise function aside while it was running, but continued running the other code, which is `console.log('second')` in this case. Then, the function resolved with the string `I have resolved`, which then got logged after it saw that the callstack was empty. 

With the await keyword in `secondFunction`, we literally pause the execution of an async function until the value has been resolved befoer moving to the next line.

This means that it waited for the `myPromise` to resolve with the value `I have resolved`, and only once that happened, we moved to the next line: `second` got logged. 

</p>
</details>

---

###### 103. What's the value of output?

```javascript
const set = new Set()

set.add(1)
set.add("Lydia")
set.add({ name: "Lydia" })

for (let item of set) {
  console.log(item + 2)
}
```

- A: `3`, `NaN`, `NaN`
- B: `3`, `7`, `NaN`
- C: `3`, `Lydia2`, `[Object object]2`
- D: `"12"`, `Lydia2`, `[Object object]2`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The `+` operator is not only used for adding numerica lvalues, but we can also use it to concatenate strings. Whenever the JavaScript engine sees that one or more values are not a number, it coerces the number into a string. 

The first one is `1`, which is a numerical value. `1 + 2` returns the number 3.

However, the second one is a string `"Lydia"`. `"Lydia"` is a string and `2` is a number: `2` gets coerced into a string. `"Lydia"` and `"2"` get concatenated, whic hresults in the string `"Lydia2"`. 

`{ name: "Lydia" }` is an object. Neither a number nor an object is a string, so it stringifies both. Whenever we stringify a regular object, it becomes `"[Object object]"`. `"[Object object]"` concatenated with `"2"` becomes `"[Object object]2"`.

</p>
</details>

---

###### 104. What's its value?

```javascript
Promise.resolve(5)
```

- A: `5`
- B: `Promise {<pending>: 5}`
- C: `Promise {<resolved>: 5}`
- D: `Error`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

We can pass any type of value we want to `Promise.resolve`, either a promise or a non-promise. The method itself returns a promise with the resolved value. If you pass a regular function, it'll be a resolved promise with a regular value. If you pass a promise, it'll be a resolved promise with the resolved value of that passed promise.

In this case, we just passed the numerical value `5`. It returns a resolved promise with the value `5`. 

</p>
</details>

---

###### 105. What's its value?

```javascript
function compareMembers(person1, person2 = person) {
  if (person1 !== person2) {
    console.log("Not the same!")
  } else {
    console.log("They are the same!")
  }
}

const person = { name: "Lydia" }

compareMembers(person)
```

- A: `Not the same!`
- B: `They are the same!`
- C: `ReferenceError`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Objects are passed by reference. When we check objects for strict equality (`===`), we're comparing their references. 

We set the default value for `person2` equal to the `person` object, and passed the `person` object as the value for `person1`.

This means that both values have a reference to the same spot in memory, thus they are equal.

The code block in the `else` statement gets run, and `They are the same!` gets logged. 

</p>
</details>

---

###### 106. What's its value?

```javascript
const colorConfig = {
  red: true,
  blue: false,
  green: true,
  black: true,
  yellow: false,
}

const colors = ["pink", "red", "blue"]

console.log(colorConfig.colors[1])
```

- A: `true`
- B: `false`
- C: `undefined`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

In JavaScript, we have two ways to access properties on an object: bracket notation, or dot notation. In this example, we use dot notation (`colorConfig.colors`) instead of bracket notation (`colorConfig["colors"]`). 

With dot notation, JavaScript tries to find the property on the object with that exact name. In this example, JavaScript tries to find a property called `colors` on the `colorConfig` object. There is no proprety called `colorConfig`, so this returns `undefined`. Then, we try to access the value of the first element by using `[1]`. We cannot do this on a value that's `undefined`, so it throws a `TypeError`: `Cannot read property '1' of undefined`.

JavaScript interprets (or unboxes) statements. When we use bracket notation, it sees the first opening bracket `[` and keeps going until it finds the closing bracket `]`. Only then, it will evaluate the statement. If we would've used `colorConfig[colors[1]]`, it would have returned the value of the `red` property on the `colorConfig` object. 

</p>
</details>

---

###### 107. What's its value?

```javascript
console.log('❤️' === '❤️')
```

- A: `true`
- B: `false`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

Under the hood, emojis are unicodes. The unicodes for the heart emoji is `"U+2764 U+FE0F"`. These are always the same for the same emojis, so we're comparing two equal strings to each other, which returns true. 

</p>
</details>

---

###### 108. Which of these methods modifies the original array?

```javascript
const emojis = ['✨', '🥑', '😍']

emojis.map(x => x + '✨')
emojis.filter(x => x !== '🥑')
emojis.find(x => x !== '🥑')
emojis.reduce((acc, cur) => acc + '✨')
emojis.slice(1, 2, '✨') 
emojis.splice(1, 2, '✨')
```

- A: `All of them`
- B: `map` `reduce` `slice` `splice`
- C: `map` `slice` `splice` 
- D: `splice`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

With `splice` method, we modify the original array by deleting, replacing or adding elements. In this case, we removed 2 items from index 1 (we removed `'🥑'` and `'😍'`) and added the ✨ emoji instead. 

`map`, `filter` and `slice` return a new array, `find` returns an element, and `reduce` returns a reduced value.

</p>
</details>

---

###### <a name=20191009></a>109. What's the output?

```javascript
const food = ['🍕', '🍫', '🥑', '🍔']
const info = { favoriteFood: food[0] }

info.favoriteFood = '🍝'

console.log(food)
```

- A: `['🍕', '🍫', '🥑', '🍔']`
- B: `['🍝', '🍫', '🥑', '🍔']`
- C: `['🍝', '🍕', '🍫', '🥑', '🍔']` 
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

We set the value of the `favoriteFood` property on the `info` object equal to the string with the pizza emoji, `'🍕'`. A string is a primitive data type. In JavaScript, primitive data types act by reference 

In JavaScript, primitive data types (everything that's not an object) interact by _value_. In this case, we set the value of the `favoriteFood` property on the `info` object equal to the value of the first element in the `food` array, the string with the pizza emoji in this case (`'🍕'`). A string is a primitive data type, and interact by value (see my [blogpost](https://www.theavocoder.com/complete-javascript/2018/12/21/by-value-vs-by-reference) if you're interested in learning more)

Then, we change the value of the `favoriteFood` property on the `info` object. The `food` array hasn't changed, since the value of `favoriteFood` was merely a _copy_ of the value of the first element in the array, and doesn't have a reference to the same spot in memory as the element on `food[0]`. When we log food, it's still the original array, `['🍕', '🍫', '🥑', '🍔']`.

</p>
</details>

---

###### 110. What does this method do?

```javascript
JSON.parse()
```

- A: Parses JSON to a JavaScript value
- B: Parses a JavaScript object to JSON
- C: Parses any JavaScript value to JSON
- D: Parses JSON to a JavaScript object only

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

With the `JSON.parse()` method, we can parse JSON string to a JavaScript value. 

```javascript
// Stringifying a number into valid JSON, then parsing the JSON string to a JavaScript value:
const jsonNumber = JSON.stringify(4) // '4'
JSON.parse(jsonNumber) // 4

// Stringifying an array value into valid JSON, then parsing the JSON string to a JavaScript value:
const jsonArray = JSON.stringify([1, 2, 3]) // '[1, 2, 3]'
JSON.parse(jsonArray) // [1, 2, 3]

// Stringifying an object  into valid JSON, then parsing the JSON string to a JavaScript value:
const jsonArray = JSON.stringify({ name: "Lydia" }) // '{"name":"Lydia"}'
JSON.parse(jsonArray) // { name: 'Lydia' }
```

</p>
</details>

---

###### 111. What's the output? 

```javascript
let name = 'Lydia'

function getName() {
  console.log(name)
  let name = 'Sarah'
}

getName()
```

- A: Lydia
- B: Sarah
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

Each function has its own _execution context_ (or _scope_). The `getName` function first looks within its own context (scope) to see if it contains the variable `name` we're trying to access. In this case, the `getName` function contains its own `name` variable: we declare the variable `name` with the `let` keyword, and with the value of `'Sarah'`. 

Variables with the `let` keyword (and `const`) are hoisted, but unlike `var`, don't get <i>initialized</i>. They are not accessible before the line we declare (initialize) them. This is called the "temporal dead zone". When we try to access the variables before they are declared, JavaScript throws a `ReferenceError`. 

If we wouldn't have declared the `name` variable within the `getName` function, the javascript engine would've looked down the _scope chain_. The outer scope has a variable called `name` with the value of `Lydia`. In that case, it would've logged `Lydia`. 

```javascript
let name = 'Lydia'

function getName() {
  console.log(name)
}

getName() // Lydia
```

</p>
</details>

---

###### 112. What's the output?

```javascript
function* generatorOne() {
  yield ['a', 'b', 'c'];
}

function* generatorTwo() {
  yield* ['a', 'b', 'c'];
}

const one = generatorOne()
const two = generatorTwo()

console.log(one.next().value)
console.log(two.next().value)
```

- A: `a` and `a`
- B: `a` and `undefined`
- C: `['a', 'b', 'c']` and `a`
- D: `a` and `['a', 'b', 'c']`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

With the `yield` keyword, we `yield` values in a generator function. With the `yield*` keyword, we can yield values from another generator function, or iterable object (for example an array).

In `generatorOne`, we yield the entire array `['a', 'b', 'c']` using the `yield` keyword. The value of `value` property on the object returned by the `next` method on `one` (`one.next().value`) is equal to the entire array `['a', 'b', 'c']`.

```javascript
console.log(one.next().value) // ['a', 'b', 'c']
console.log(one.next().value) // undefined
```

In `generatorTwo`, we use the `yield*` keyword. This means that the first yielded value of `two`, is equal to the first yielded value in the iterator. The iterator is the array `['a', 'b', 'c']`. The first yielded value is `a`, so the first time we call `two.next().value`, `a` is returned. 

```javascript
console.log(two.next().value) // 'a'
console.log(two.next().value) // 'b'
console.log(two.next().value) // 'c'
console.log(two.next().value) // undefined
```

</p>
</details>

---

###### 113. What's the output?

```javascript
console.log(`${(x => x)('I love')} to program`)
```

- A: `I love to program`
- B: `undefined to program`
- C: `${(x => x)('I love') to program`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

Expressions within template literals are evaluated first. This means that the string will contain the returned value of the expression, the immediately invoked function `(x => x)('I love')` in this case. We pass the value `'I love'` as an argument to the `x => x` arrow function. `x` is equal to `'I love'`, which gets returned. This results in `I love to program`. 

</p>
</details>

---

###### 114. What will happen?

```javascript
let config = {
  alert: setInterval(() => {
    console.log('Alert!')
  }, 1000)
}

config = null
```

- A: The `setInterval` callback won't be invoked
- B: The `setInterval` callback gets invoked once
- C: The `setInterval` callback will still be called every second
- D: We never invoked `config.alert()`, config is `null`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Normally when we set objects equal to `null`, those objects get _garbage collected_ as there is no reference anymore to that object. However, since the callback function within `setInterval` is an arrow function (thus bound to the `config` object), the callback function still holds a reference to the `config` object. As long as there is a reference, the object won't get garbage collected. Since it's not garbage collected, the `setInterval` callback function will still get invoked every 1000ms (1s).

</p>
</details>

---

###### 115. Which method(s) will return the value `'Hello world!'`?

```javascript
const myMap = new Map()
const myFunc = () => 'greeting'

myMap.set(myFunc, 'Hello world!')

//1
myMap.get('greeting')
//2
myMap.get(myFunc)
//3
myMap.get(() => 'greeting')
```

- A: 1
- B: 2
- C: 2 and 3
- D: All of them

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

When adding a key/value pair using the `set` method, the key will be the value of the first argument passed to the `set` function, and the value will be the second argument passed to the `set` function. The key is the _function_ `() => 'greeting'` in this case, and the value `'Hello world'`. `myMap` is now `{ () => 'greeting' => 'Hello world!' }`. 

1 is wrong, since the key is not `'greeting'` but `() => 'greeting'`.
3 is wrong, since we're creating a new function by passing it as a parameter to the `get` method. Object interact by _reference_. Functions are objects, which is why two functions are never strictly equal, even if they are identical: they have a reference to a different spot in memory. 

</p>
</details>

---

###### 116. What's the output?

```javascript
const person = {
  name: "Lydia",
  age: 21
}

const changeAge = (x = { ...person }) => x.age += 1
const changeAgeAndName = (x = { ...person }) => {
  x.age += 1
  x.name = "Sarah"
}

changeAge(person)
changeAgeAndName()

console.log(person)
```

- A: `{name: "Sarah", age: 22}`
- B: `{name: "Sarah", age: 23}`
- C: `{name: "Lydia", age: 22}`
- D: `{name: "Lydia", age: 23}`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Both the `changeAge` and `changeAgeAndName` functions have a default parameter, namely a _newly_ created object `{ ...person }`. This object has copies of all the key/values in the `person` object. 
