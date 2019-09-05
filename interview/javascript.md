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
    
