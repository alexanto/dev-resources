## HTML

* **What does a doctype do?**

  doctype is an abbreviation for document type. It is a declaration used in HTML5 to distinguish between a standards-compliant parsing mode and a quirks parsing mode. Hence its presence tells the browser to parse and render the webpage in standards mode.
  Moral of the story, just add <!DOCTYPE html> to the start of your page.

  **References**
  * https://stackoverflow.com/questions/7695044/what-does-doctype-html-do
  * https://www.w3.org/QA/Tips/Doctype
  
* **What’s the difference between full standards mode, almost standards mode and quirks mode?**
  Quirks mode — Layout emulates non-standard behavior in Netscape Navigator 4 and Internet Explorer 5. This is essential in order to support websites that were built before the widespread adoption of web standards. The list of quirks can be found here.
  Full standards mode — The layout behavior is the one described by the HTML and CSS specifications.
  Almost standards mode — There are only a very small number of quirks implemented. Differences can be found here.
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode
  
* **What’s the difference between HTML and XHTML?**
  XHTML belongs to the family of XML markups languages and is different from HTML. Some of differences are as follows:
  
  * XHTML documents have to be well-formed, unlike HTML, which is more forgiving.
  * XHTML is case-sensitive for element and attribute names, while HTML is not.
  * Raw < and & characters are not allowed except inside of CDATA Sections (<![CDATA[ ... ]]>). JavaScript typically contains characters which can not exist in XHTML outside of CDATA Sections, such as the < operator. Hence it is tricky to use inline styles or script tags in XHTML and should be avoided.
  * A fatal parse error in XML (such as an incorrect tag structure) causes document processing to be aborted.
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Archive/Web/Properly_Using_CSS_and_JavaScript_in_XHTML_Documents_
  * https://en.wikipedia.org/wiki/XHTML
  
* **Are there any problems with serving pages as application/xhtml+xml?**

  Basically the problems lie in the differences between parsing HTML and XML as mentioned above.
  
  * XHTML, or rather, XML syntax is less forgiving and if your page isn’t fully XML-compliant, there will be parsing errors and users get unreadable content.
  * Serving your pages as application/xhtml+xml will cause Internet Explorer 8 to show a download dialog box for an unknown format instead of displaying your page, as the first version of Internet Explorer with support for XHTML is Internet Explorer 9.
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode#XHTML
  
* **How do you serve a page with content in multiple languages?**
  The question is a little vague, I will assume that it is asking about the most common case, which is how to serve a page with content available in multiple languages, but the content within the page is only in a single language.
  When an HTTP request is made to a server, the requesting user agent usually sends information about language preferences, such as in the Accept-Language header. The server can then use this information to return a version of the document in the appropriate language if such an alternative is available. The returned HTML document should also declare the lang attribute in the <html> tag, such as <html lang="en">...</html>.
  In the back end, the HTML markup will contain i18n placeholders and content for the specific language stored in YML or JSON formats. The server then dynamically generates the HTML page with content in that particular language, usually with the help of a back end framework.
  
  **References**
  * https://www.w3.org/International/getting-started/language
  
* **What kind of things must you be wary of when designing or developing for multilingual sites?**

  * Use lang attribute in your HTML.
  * Directing users to their native language — Allow a user to change his country/language easily without hassle.
  * Text in images is not a scalable approach — Placing text in an image is still a popular way to get good-looking, non-system fonts to display on any computer. However to translate image text, each string of text will need to have it’s a separate image created for each language. Anything more than a handful of replacements like this can quickly get out of control.
  * Restrictive words / sentence length — Some content can be longer when written in another language. Be wary of layout or overflow issues in the design. It’s best to avoid designing where the amount of text would make or break a design. Character counts come into play with things like headlines, labels, and buttons. They are less of an issue with free flowing text such as body text or comments.
  * Be mindful of how colors are perceived — Colors are perceived differently across languages and cultures. The design should use color appropriately.
  * Formatting dates and currencies — Calendar dates are sometimes presented in different ways. Eg. “May 31, 2012” in the U.S. vs. “31 May 2012” in parts of Europe.
  * Do not concatenate translated strings — Do not do anything like "The date today is " + date. It will break in languages with different word order. Using template parameters instead.
  
  **References**
  * https://www.quora.com/What-kind-of-things-one-should-be-wary-of-when-designing-or-developing-for-multilingual-sites
  
* **What are data- attributes good for?**

  Before JavaScript frameworks became popular, front end developers used data- attributes to store extra data within the DOM itself, without other hacks such as non-standard attributes, extra properties on the DOM. It is intended to store custom data private to the page or application, for which there are no more appropriate attributes or elements.
  These days, using data- attributes is not encouraged. For one thing, users can modify the data attribute easily by using inspect element in the browser. The data model is better stored within JavaScript itself and stay updated with the DOM via data binding possibly through a library or a framework.
  
  **References**
  * http://html5doctor.com/html5-custom-data-attributes/
  
* **Consider HTML5 as an open web platform. What are the building blocks of HTML5?**

  * Semantics — Allowing you to describe more precisely what your content is.
  * Connectivity — Allowing you to communicate with the server in new and innovative ways.
  * Offline and storage — Allowing webpages to store data on the client-side locally and operate offline more efficiently.
  * Multimedia — Making video and audio first-class citizens in the Open Web.
  * 2D/3D graphics and effects — Allowing a much more diverse range of presentation options.
  * Performance and integration — Providing greater speed optimization and better usage of computer hardware.
  * Device access — Allowing for the usage of various input and output devices.
  * Styling — Letting authors write more sophisticated themes.
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5
  
* **Describe the difference between a cookie, sessionStorage and localStorage.**

  All the above mentioned technologies are key-value storage mechanisms on the client side. They are only able to store values as strings.
  
  *cookie*
  * Initiator — Client or server. Server can use Set-Cookieheader
  * Expiry — Manually set
  * Persistent across browser sessions — 
  * Depends on whether expiration is set
  * Have domain associated — Yes
  * Sent to server with every HTTP request — Yes
  * Capacity (per domain) — 4kb
  * Accessibility — Any window
  
  *localStorage*
  * Initiator — Client
  * Expiry — Forever
  * Persistent across browser sessions — Yes
  * Have domain associated — No
  * Sent to server with every HTTP request — No
  * Capacity (per domain) — 5MB
  * Accessibility — Any window
  
  *sessionStorage*
  * Initiator — Client
  * Expiry — On tab close
  * Persistent across browser sessions — No
  * Have domain associated — No
  * Sent to server with every HTTP request — No
  * Capacity (per domain) — 5MB
  * Accessibility — Same tab
  
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
  * http://tutorial.techaltum.com/local-and-session-storage.html


* **Describe the difference between `<script>`, `<script async>` and `<script defer>`.**
  * `<script>` - HTML parsing is blocked, the script is fetched and executed immediately, HTML parsing resumes after the script is executed.
  * `<script async>` - The script will be fetched in parallel to HTML parsing and executed as soon as it is available (potentially before HTML parsing completes). Use async when the script is independent of any other scripts on the page, for example analytics.
  * `<script defer>` - The script will be fetched in parallel to HTML parsing and executed when the page has finished parsing. If there are multiple of them, each deferred script is executed in the order they were encoun­tered in the document. If a script relies on a fully-parsed DOM, the defer attribute will be useful in ensuring that the HTML is fully parsed before executing. There's not much difference from putting a normal <script> at the end of <body>. A deferred script must not contain document.write.
  
  **References**
  * http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html
  * https://stackoverflow.com/questions/10808109/script-tag-async-defer
  * https://bitsofco.de/async-vs-defer/
  Note: The async and defer attrib­utes are ignored for scripts that have no src attribute.
  
* **Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?**
  
    Placing `<link>`s in the `<head>`— Putting `<link>`s in the head is part of the specification. Besides that, placing at the top allows the page to render progressively which improves user experience. The problem with putting stylesheets near the bottom of the document is that it prohibits progressive rendering in many browsers, including Internet Explorer. Some browsers block rendering to avoid having to repaint elements of the page if their styles change. The user is stuck viewing a blank white page. It prevents the flash of unstyled contents.
  
   Placing `<scripts>`s just before `</body>` — `<script>`s block HTML parsing while they are being downloaded and executed. Downloading the scripts at the bottom will allow the HTML to be parsed and displayed to the user first.

   An exception for positioning of <script>s at the bottom is when your script contains document.write(), but these days it's not a good practice to use document.write(). Also, placing <script>s at the bottom means that the browser cannot start downloading the scripts until the entire document is parsed. One possible workaround is to put `<script>`in the `<head>` and use the defer attribute.
  
    **References**
    * https://developer.yahoo.com/performance/rules.html#css_top
   
* **What is progressive rendering?**

  Progressive rendering is the name given to techniques used to improve performance of a webpage (in particular, improve perceived load time) to render content for display as quickly as possible.
  
  It used to be much more prevalent in the days before broadband internet but it is still useful in modern development as mobile data connections are becoming increasingly popular (and unreliable)!
  
  Examples of such techniques:
  
  * Lazy loading of images — Images on the page are not loaded all at once. JavaScript will be used to load an image when the user scrolls into the part of the page that displays the image.
  * Prioritizing visible content (or above-the-fold rendering) — Include only the minimum CSS/content/scripts necessary for the amount of page that would be rendered in the users browser first to display as quickly as possible, you can then use deferred scripts or listen for the DOMContentLoaded/load event to load in other resources and content.
  * Async HTML fragments — Flushing parts of the HTML to the browser as the page is constructed on the back end. More details on the technique can be found here.
  
  **References**
  * https://stackoverflow.com/questions/33651166/what-is-progressive-rendering
  * http://www.ebaytechblog.com/2014/12/08/async-fragments-rediscovering-progressive-html-rendering-with-marko/
  
* **Have you used different HTML templating languages before?**

  Yes, Jade, ERB, Slim, Handlebars, Jinja, Liquid, just to name a few. In my opinion, they are more or less the same and provide similar functionality of escaping content and helpful filters for manipulating the data to be displayed. Most templating engines will also allow you to inject your own filters in the event you need custom processing before display.
  
* **What is the purpose of the alt attribute on images?**

  The `alt` attribute provides alternative information for an image if a user cannot view it. The `alt` attribute should be used to describe any images except those which only serve a decorative purpose, in which case it should be left empty.

  **Good to hear**
  * Decorative images should have an empty alt attribute.

  * Web crawlers use alt tags to understand image content, so they are considered important for Search Engine Optimization (SEO).

  * Put the . at the end of alt tag to improve accessibility.
  
* **Can a web page contain multiple `<header>` elements? What about `<footer>` elements?**
  
    Yes to both. The W3 documents state that the tags represent the header(`<header>`) and footer(`<footer>`) areas of their nearest ancestor "section". So not only can the page `<body>` contain a header and a footer, but so can every `<article>` and `<section>` element.

  **Good to hear**
    * W3 recommends having as many as you want, but only 1 of each for each "section" of your page, i.e. body, section etc.
W3 recommends having as many as you want, but only 1 of each for each "section" of your page, i.e. body, section etc.
