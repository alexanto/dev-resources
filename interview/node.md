## Node

* **When are background/worker processes useful? How can you handle worker tasks?**

  Worker processes are extremely useful if you'd like to do data processing in the background, like sending out emails or processing images.

* **How can you secure your HTTP cookies against XSS attacks?**
  
  XSS occurs when the attacker injects executable JavaScript code into the HTML response.

  To mitigate these attacks, you have to set flags on the set-cookie HTTP header:

  * **HttpOnly** - this attribute is used to help prevent attacks such as cross-site scripting since it does not allow the cookie to be accessed via JavaScript.
  * **secure** - this attribute tells the browser to only send the cookie if the request is being sent over HTTPS.
  
     So it would look something like this: `Set-Cookie: sid=<cookie-value>; HttpOnly`. If you are using Express, with express-cookie session, it is working by default.

* **How can you make sure your dependencies are safe?**

    The only option is to automate the update / security audit of your dependencies. For that there are free and paid options:

   * npm outdated
   * Trace by RisingStack
   * NSP
   * GreenKeeper
   * Snyk

* **What's wrong with the code snippet?**

  ```
  new Promise((resolve, reject) => {
    throw new Error('error')
  }).then(console.log)
  ```

    As there is no catch after the then. This way the error will be a silent one, there will be no indication of an error thrown.

    To fix it, you can do the following:

  ```
  new Promise((resolve, reject) => {
    throw new Error('error')
  }).then(console.log).catch(console.error)
  ```
  
  If you have to debug a huge codebase, and you don't know which Promise can potentially hide an issue, you can use the unhandledRejection hook. It will print out all unhandled Promise rejections.

  ```
  process.on('unhandledRejection', (err) => {
    console.log(err)
  })
  ```
  
* **What's wrong with the following code snippet?**

  ```
  function checkApiKey (apiKeyFromDb, apiKeyReceived) {
    if (apiKeyFromDb === apiKeyReceived) {
      return true
    }
    return false
  }
  ```

     When you compare security credentials it is crucial that you don't leak any information, so you have to make sure that you compare them in fixed time. If you fail to do so, your application will be vulnerable to timing attacks.

     But why does it work like that?

      V8, the JavaScript engine used by Node.js, tries to optimize the code you run from a performance point of view. It starts comparing the strings character by character, and once a mismatch is found, it stops the comparison operation. So the longer the attacker has right from the password, the more time it takes.
     
     To solve this issue, you can use the npm module called cryptiles.
     
     ```
    function checkApiKey (apiKeyFromDb, apiKeyReceived) {
      return cryptiles.fixedTimeComparison(apiKeyFromDb, apiKeyReceived)
    }
    ```
     
 * **What's the output of following code snippet?**
     
     The short answer is 2 - however with this question I'd recommend asking the candidates to explain what will happen line-by-line to understand how they think. It should be something like this:

    1. A new Promise is created, that will resolve to 1.
    2. The resolved value is incremented with 1 (so it is 2 now), and returned instantly.
    3. The resolved value is discarded, and an error is thrown.
    4. The error is discarded, and a new value (1) is returned.
    5. The execution did not stop after the catch, but before the exception was handled, it continued, and a new, incremented value (2) is returned.
    6. The value is printed to the standard output.
    7. This line won't run, as there was no exception.
