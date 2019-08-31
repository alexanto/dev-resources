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
