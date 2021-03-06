## CSS

* **What is the difference between classes and IDs in CSS?**

  * **IDs** — Meant to be unique within the document. Can be used to identify an element when linking using a fragment identifier. Elements can only have one `id` attribute.
  * **Classes** — Can be reused on multiple elements within the document. Mainly for styling and targeting elements.
  
* **What’s the difference between “resetting” and “normalizing” CSS? Which would you choose, and why?**

  * Resetting — Resetting is meant to strip all default browser styling on elements. For e.g `margins`, `paddings`, `font-size`s of all elements are reset to be the same. You will have to redeclare styling for common typographic elements.
  * Normalizing — Normalizing preserves useful default styles rather than “unstyling” everything. It also corrects bugs for common browser dependencies.
  
    I would choose resetting when I have very a customized or unconventional site design such that I need to do a lot of my own styling do not need any default styling to be preserved.
    
    **References**
    * https://stackoverflow.com/questions/6887336/what-is-the-difference-between-normalize-css-and-reset-css
    
* **Describe `float`s and how they work.**

  Float is a CSS positioning property. Floated elements remain a part of the flow of the page, and will affect the positioning of other elements (e.g. text will flow around floated elements), unlike `position: absolute` elements, which are removed from the flow of the page.
  
  The CSS `clear` property can be used to be positioned below `left`/`right`/`both` floated elements.
  
  If a parent element contains nothing but floated elements, its height will be collapsed to nothing. It can be fixed by clearing the float after the floated elements in the container but before the close of the container.
  
  The `.clearfix` hack uses a clever CSS pseudo selector (:after) to clear floats. Rather than setting the overflow on the parent, you apply an additional class like `clearfix` to it. Then apply this CSS:
  
  ```
    .clearfix:after {
      content: '.';
      visibility: hidden;
      display: block;
      height: 0;
      clear: both;
    }
  ```
  
  Alternatively, give `overflow: auto` or `overflow: hidden` property to the parent element which will establish a new block formatting context inside the children and it will expand to contain its children.
  
  **References**
  * https://css-tricks.com/all-about-floats/
  
* **Describe `z-index` and how stacking context is formed.**

  The `z-index` property in CSS controls the vertical stacking order of elements that overlap. `z-index` only effects elements that have a position value which is not `static`.
  
  Without any `z-index` value, elements stack in the order that they appear in the DOM (the lowest one down at the same hierarchy level appears on top). Elements with non-static positioning (and their children) will always appear on top of elements with default static positioning, regardless of HTML hierarchy.
  
  A stacking context is an element that contains a set of layers. Within a local stacking context, the `z-index` values of its children are set relative to that element rather than to the document root. Layers outside of that context — i.e. sibling elements of a local stacking context — can't sit between layers within it. If an element B sits on top of element A, a child element of element A, element C, can never be higher than element B even if element C has a higher `z-index` than element B.
  
  Each stacking context is self-contained — after the element’s contents are stacked, the whole element is considered in the stacking order of the parent stacking context. A handful of CSS properties trigger a new stacking context, such as `opacity` less than 1, `filter` that is not `none`, and `transform` that is notnone`.`
  
  **References**
  * https://css-tricks.com/almanac/properties/z/z-index/
  * https://philipwalton.com/articles/what-no-one-told-you-about-z-index/
  * https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context
  
* **Describe Block Formatting Context (BFC) and how it works.**

  A Block Formatting Context (BFC) is part of the visual CSS rendering of a web page in which block boxes are laid out. Floats, absolutely positioned elements, `inline-blocks`, `table-cells`, `table-captions`, and elements with `overflow` other than `visible` (except when that value has been propagated to the viewport) establish new block formatting contexts.
  
  A BFC is an HTML box that satisfies at least one of the following conditions:
  
  * The value of `float` is not `none`.
  * The value of `position` is neither `static` nor `relative`.
  * The value of `display` is `table-cell`, `table-caption`, `inline-block`, `flex`, or `inline-flex`.
  * The value of `overflow` is not `visible`.
  
  In a BFC, each box’s left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch).
  
  Vertical margins between adjacent block-level boxes in a BFC collapse. Read more on collapsing margins.
  
  **References**
  * https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context
  * https://www.sitepoint.com/understanding-block-formatting-contexts-in-css/
  
 * **What are the various clearing techniques and which is appropriate for what context?**
 
   * Empty `div` method - `<div style="clear:both;"></div>`.
   * Clearfix method — Refer to the `.clearfix` class above.
   * `overflow: auto` or `overflow: hidden` method - Parent will establish a new block formatting context and expand to contains its floated children.
 
   * In large projects, I would write a utility .clearfix class and use them in places where I need it. overflow: hiddenmight clip children if the children is taller than the parent and is not very ideal.
 
* **Explain CSS sprites, and how you would implement them on a page or site.**
 
  CSS sprites combine multiple images into one single larger image. It is commonly used technique for icons (Gmail uses it). How to implement it:
 
  1. Use a sprite generator that packs multiple images into one and generate the appropriate CSS for it.
  2. Each image would have a corresponding CSS class with background-image, background-position and background-size properties defined.
  3. To use that image, add the corresponding class to your element.
 
  **Advantages:**
 
  * Reduce the number of HTTP requests for multiple images (only one single request is required per spritesheet). But with HTTP2, loading multiple images is no longer much of an issue.
  * Advance downloading of assets that won’t be downloaded until needed, such as images that only appear upon:hover pseudo-states. Blinking wouldn't be seen.
 
* **What are your favorite image replacement techniques and which do you use when?**

  CSS image replacement is a technique of replacing a text element (usually a header tag like an `<h1>`) with an image (often a logo). It has its origins in the time before web fonts and SVG. For years, web developers battled against browser inconsistencies to craft image replacement techniques that struck the right balance between design and accessibility.
 
  It’s not really relevant these days. Check out this link for all the available techniques.
 
   **References**
   * https://css-tricks.com/the-image-replacement-museum/
 
* **How would you approach fixing browser-specific styling issues?**

   * After identifying the issue and the offending browser, use a separate style sheet that only loads when that specific browser is being used. This technique requires server-side rendering though.
   * Use libraries like Bootstrap that already handles these styling issues for you.
   * Use autoprefixer to automatically add vendor prefixes to your code.
   * Use Reset CSS or Normalize.css.
 
* **How do you serve your pages for feature-constrained browsers? What techniques/processes do you use?**
 
   * Graceful degradation — The practice of building an application for modern browsers while ensuring it remains functional in older browsers.
   * Progressive enhancement — The practice of building an application for a base level of user experience, but adding functional enhancements when a browser supports it.
   * Use caniuse.com to check for feature support.
   * Autoprefixer for automatic vendor prefix insertion.
   * Feature detection using Modernizr.
 
* **What are the different ways to visually hide content (and make it available only for screen readers)?**
 
  These techniques are related to accessibility (a11y).
 
  * `visibility: hidden`. However the element is still in the flow of the page, and still takes up space.
  * `width: 0; height: 0`. Make the element not take up any space on the screen at all, resulting in not showing it.
  * `position; absolute; left: -99999px`. Position it outside of the screen.
  * `text-indent: -9999px`. This only works on text within the block elements.
 
  I would go with the `absolute` positioning approach, as it has the least caveats and works for most elements.
 
* **Have you ever used a grid system, and if so, what do you prefer?**

  I like the `float`-based grid system because it still has the most browser support among the alternative existing systems (flex, grid). It has been used in for Bootstrap for years and has been proven to work.
 
* **Have you used or implemented media queries or mobile-specific layouts/CSS?**

  Yes. An example would be transforming a stacked pill navigation into a fixed-bottom tab navigation beyond a certain breakpoint.
 
* **How do you optimize your webpages for print?**

  * Create a stylesheet for print or use media queries.
 
 ```
 <!-- Main stylesheet on top -->
 <link rel="stylesheet" href="/global.css" media="all" />
 <!-- Print only, on bottom -->
 <link rel="stylesheet" href="/print.css" media="print" />
 ```
 
 Make sure to put non-print styles inside `@media screen { ... }`.
 
 ```
 @media print {
   ...
 }
 ```
 
 * Deliberately add page breaks.
 
 ```
 <style>
 .page-break { 
   display: none;
   page-break-before: always; 
 }
 </style>
 Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Fusce eu felis. Curabitur sit amet magna. Nullam aliquet. Aliquam ut diam...
 <div class="page-break"></div>
 Lorem ipsum dolor sit amet, consectetuer adipiscing elit....
 ```
 
 **References**
 * https://davidwalsh.name/optimizing-structure-print-css
 
* **What are some of the “gotchas” for writing efficient CSS?**

  Firstly, understand that browsers match selectors from rightmost (key selector) to left. Browsers filter out elements in the DOM according to the key selector, and traverse up its parent elements to determine matches. The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector. Hence avoid key selectors that are tag and universal selectors. They match a large numbers of elements and browsers will have to do more work in determining if the parents do match.
 
  BEM (Block Element Modifier) methodology recommends that everything has a single class, and, where you need hierarchy, that gets baked into the name of the class as well, this naturally makes the selector efficient and easy to override.
 
  Be aware of which CSS properties trigger reflow, repaint and compositing. Avoid writing styles that change the layout (trigger reflow) where possible.
 
  **References**
   * https://developers.google.com/web/fundamentals/performance/rendering/
   * https://csstriggers.com/
 
* **What are the advantages/disadvantages of using CSS preprocessors?**

  **Advantages:**
   * CSS is made more maintainable.
   * Easy to write nested selectors.
   * Variables for consistent theming. Can share theme files across different projects.
   * Mixins to generate repeated CSS.
   * Splitting your code into multiple files. CSS files can be split up too but doing so will require a HTTP request to download each CSS file.
 
  **Disadvantages:**
   * Requires tools for preprocessing. Re-compilation time can be slow.
 
* **Describe what you like and dislike about the CSS preprocessors you have used.**

  **Likes:**
   * Mostly the advantages mentioned above.
   * Less is written in JavaScript, which plays well with Node.
 
  **Dislikes:**
   * I use Sass via node-sass, which is a binding for LibSass, which is written in C++. Have to frequently recompile it when switching between node versions.
  * In Less, variable names are prefixed with @, which can be confused with native CSS keywords like @media, @import and @font-face rule.

* **How would you implement a web design comp that uses non-standard fonts?**
  Use `@font-face` and define `font-family` for different font-weights.
  
 * **Explain how a browser determines what elements match a CSS selector.**
 
   This part is related to the above about writing efficient CSS. Browsers match selectors from rightmost (key selector) to left. Browsers filter out elements in the DOM according to the key selector, and traverse up its parent elements to determine matches. The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector.
  
   For example with this selector p span, browsers firstly find all the `<span>` elements, and traverse up its parent all the way up to the root to find the `<p>` element. For a particular <span>, as soon as it finds a `<p>`, it knows that the `<span>` matches and can stop its matching.
 
 **References**
 * https://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left
 
* **Describe pseudo-elements and discuss what they are used for.**

  A CSS pseudo-element is a keyword added to a selector that lets you style a specific part of the selected element(s). They can be used for decoration (`:first-line`, `:first-letter`) or adding elements to the markup (combined with `content: ...`) without having to modify the markup (`:before, :after`).
 
  * `:first-line` and `:first-letter` can be used to decorate text.
  * Used in the `.clearfix` hack as shown above to add a zero-space element with `clear: both`.
  * Triangular arrows in tooltips use `:before` and `:after`. Encourages separation of concerns because the triangle is considered part of styling and not really the DOM, but not really possible to draw a triangle with just CSS styles.
  **References**
  * https://css-tricks.com/almanac/selectors/a/after-and-before/
 
* **Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.**

  The CSS box model is responsible for calculating:
  * How much space a block-level element takes up.
  * Whether or not borders and/or margins overlap, or collapse.
  * A box’s dimensions.

 The box model has the following rules:

  * The dimensions of a block element are calculated by `width`, `height`, `padding`, `borders`, and `margins`.
  * If no height is specified, a `block` element will be as high as the content it contains, plus padding (unless there are floats, for which see below).
  * If no width is specified, a non-floated `block` element will expand to fit the width of its parent minus padding.
  * The `height` of an element is calculated by the content's height.
  * The `width` of an element is calculated by the content's width.
  * By default, `padding`s and `border`s are not part of the `width` and `height` of an element.
 
  **References**
  * https://www.smashingmagazine.com/2010/06/the-principles-of-cross-browser-css-coding/#understand-the-css-box-model
 
* **What does * { box-sizing: border-box; } do? What are its advantages?**

  * By default, elements have `box-sizing: content-box` applied, and only the content size is being accounted for.
  * `box-sizing: border-box` changes how the `width` and `height` of elements are being calculated, `border` and `padding` are also being included in the calculation.
  * The `height` of an element is now calculated by the content's `height` + vertical `padding` + vertical `border` width.
  * The `width` of an element is now calculated by the content's width + horizontal `padding` + horizontal `borderwidth`.
 
* **List as many values for the `display` property that you can remember.**
 
   * `none`, `block`, `inline`, `inline-block`, `table`, `table-row`, `table-cell`, `list-item`.
  
 * **What’s the difference between `inline` and `inline-block`?**
 
   I shall throw in a comparison with block for good measure.
 
    **Block**
    * Size — Fills up the width of its parent container.
    * Positioning — Start on a new line and tolerates no HTML elements next to it (except when you add `float`).
    * Can specify `width` and `height` — Yes.
    * Can be aligned with `vertical-align` — Yes.
    * Margins and paddings — All sides respected.

    **Inline-Block**
    * Size — Depends on content.
    * Positioning — Flows along with other content and allows other elements beside.
    * Can specify `width` and `height` — Yes.
    * Can be aligned with `vertical-align` — Yes.
    * Margins and paddings — All sides respected.

     **Inline**
     * Size — Depends on content.
     * Positioning — Flows along with other content and allows other elements beside.
     * Can specify `width` and `height` — No. Will ignore if being set.
     * Can be aligned with `vertical-align` — Only horizontal sides respected. Vertical sides, if specified, do not affect layout. Vertical space it takes up depends on `line-height`, even though the `border` and `padding` appear visually around the content.
    * Margins and paddings — Becomes like a `block` element where you can set vertical margins and paddings.

* **What’s the difference between a `relative`, `fixed`, `absolute` and `static`-ally positioned element?**

  A positioned element is an element whose computed `position` property is either `relative`, `absolute`, `fixed` or `sticky`.
 
    * `static` - The default position; the element will flow into the page as it normally would. The top, right, bottom, left and z-index properties do not apply.
    * `relative` - The element's position is adjusted relative to itself, without changing layout (and thus leaving a gap for the element where it would have been had it not been positioned).
   * `absolute` - The element is removed from the flow of the page and positioned at a specified position relative to its closest positioned ancestor if any, or otherwise relative to the initial containing block. Absolutely positioned boxes can have margins, and they do not collapse with any other margins. These elements do not affect the position of other elements.
   * `fixed` - The element is removed from the flow of the page and positioned at a specified position relative to the viewport and doesn't move when scrolled.
    * `sticky` - Sticky positioning is a hybrid of relative and fixed positioning. The element is treated as relative positioned until it crosses a specified threshold, at which point it is treated as fixed positioned.
 
  **References**
  * https://developer.mozilla.org/en/docs/Web/CSS/position
 
* **The ‘C’ in CSS stands for Cascading. How is priority determined in assigning styles (a few examples)? How can you use this system to your advantage?**

  Browser determines what styles to show on an element depending on the specificity of CSS rules. We assume that the browser has already determined the rules that match a particular element. Among the matching rules, the specificity, four comma-separate values, `a, b, c, d` are calculated for each rule based on the following:
 
  1. `a` is whether inline styles are being used. If the property declaration is an inline style on the element, `a` is 1, else 0.
  2. `b` is the number of ID selectors.
  3. `c` is the number of classes, attributes and pseudo-classes selectors.
  4. `d` is the number of tags and pseudo-elements selectors.
 
 
  The resulting specificity is not a score, but a matrix of values that can be compared column by column. When comparing selectors to determine which has the highest specificity, look from left to right, and compare the highest value in each column. So a value in column `b` will override values in columns `c` and d, no matter what they might be. As such, specificity of `0,1,0,0` would be greater than one of `0,0,10,10`.
 
  In the cases of equal specificity: the latest rule is the one that counts. If you have written the same rule into your style sheet (regardless of internal or external) twice, then the lower rule in your style sheet is closer to the element to be styled, it is deemed to be more specific and therefore will be applied.
 
  I would write CSS rules with low specificity so that they can be easily overridden if necessary. When writing CSS UI component library code, it is important that they have low specificities so that users of the library can override them without using too complicated CSS rules just for the sake of increasing specificity or resorting to `!important`.
 
  **References**
  * https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/
  * https://www.sitepoint.com/web-foundations/specificity/
 
* **What existing CSS frameworks have you used locally, or in production? How would you change/improve them?**

   * **Bootstrap** — Slow release cycle. Bootstrap 4 has been in alpha for almost 2 years. Add a spinner button component, as it is widely-used.
   * **Semantic UI** — Source code structure makes theme customization is extremely hard to understand. Painful to customize with unconventional theming system. Hardcoded config path within the vendor library. Not well-designed for overriding variables unlike in Bootstrap.
   * **Bulma** — A lot of non-semantic and superfluous classes and markup required. Not backward compatible. Upgrading versions breaks the app in subtle manners.
  
* **Have you played around with the new CSS Flexbox or Grid specs?**

  Yes. Flexbox is mainly meant for 1-dimensional layouts while Grid is meant for 2-dimensional layouts.

  Flexbox solves many common problems in CSS, such as vertical centering of elements within a container, sticky footer, etc. Bootstrap and Bulma are based on Flexbox, and it is probably the recommended way to create layouts these days. Have tried Flexbox before but ran into some browser incompatibility issues (Safari) in using `flex-grow`, and I had to rewrite my code using `inline-blocks` and math to calculate the widths in percentages, it wasn’t a nice experience.
 
  Grid is by far the most intuitive approach for creating grid-based layouts (it better be!) but browser support is not wide at the moment.

  **References**
  * https://philipwalton.github.io/solved-by-flexbox/
  * https://css-tricks.com/snippets/css/a-guide-to-flexbox/
  * https://css-tricks.com/snippets/css/complete-guide-grid/
  
* **How is responsive design different from adaptive design?**

   Both responsive and adaptive design attempt to optimize the user experience across different devices, adjusting for different viewport sizes, resolutions, usage contexts, control mechanisms, and so on.

  Responsive design works on the principle of flexibility — a single fluid website that can look good on any device. Responsive websites use media queries, flexible grids, and responsive images to create a user experience that flexes and changes based on a multitude of factors. Like a single ball growing or shrinking to fit through several different hoops.

  Adaptive design is more like the modern definition of progressive enhancement. Instead of one flexible design, adaptive design detects the device and other features, and then provides the appropriate feature and layout based on a predefined set of viewport sizes and other characteristics. The site detects the type of device used, and delivers the pre-set layout for that device. Instead of a single ball going through several different-sized hoops, you’d have several different balls to use depending on the hoop size.
 
  **References**
  * https://developer.mozilla.org/en-US/docs/Archive/Apps/Design/UI_layout_basics/Responsive_design_versus_adaptive_design
  * http://mediumwell.com/responsive-adaptive-mobile/
  * https://css-tricks.com/the-difference-between-responsive-and-adaptive-design/
  
* **Have you ever worked with retina graphics? If so, when and what techniques did you use?**

  I tend to use higher resolution graphics (twice the display size) to handle retina display. The better way would be to use a media query like @media only screen and (min-device-pixel-ratio: 2) { ... } and change the background-image.
 
  For icons, I would also opt to use svgs and icon fonts where possible, as they render very crisply regardless of resolution.
 
  Another method would be to use JavaScript to replace the <img> src attribute with higher resolution versions after checking the window.devicePixelRatio value.
  
  **References**
  * https://www.sitepoint.com/css-techniques-for-retina-displays/
  
* **Is there any reason you’d want to use `translate()` instead of `absolute` positioning, or vice-versa? And why?**

  `translate()` is a value of CSS `transform`. Changing `transform` or `opacity` does not trigger browser `reflow` or repaint, only compositions, whereas changing the absolute positioning triggers `reflow`. `transform` causes the browser to create a GPU layer for the element but changing absolute positioning properties uses the CPU. Hence translate() is more efficient and will result in shorter paint times for smoother animations.

  When using `translate()`, the element still takes up its original space (sort of like `position: relative`), unlike in changing the absolute positioning.

  **References**
  * https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/
  
* **What is CSS BEM?**

 The BEM methodology is a naming convention for CSS classes in order to keep CSS more maintainable by defining namespaces to solve scoping issues. BEM stands for Block Element Modifier which is an explanation for its structure. A Block is a standalone component that is reusable across projects and acts as a "namespace" for sub components (Elements). Modifiers are used as flags when a Block or Element is in a certain state or is different in structure or style.
 
 ```
 /* block component */
 .block {
 }

 /* element */
 .block__element {
 }

 /* modifier */
 .block__element--modifier {
 }
 ```
 
 Here is an example with the class names on markup:
 
 ```
 <nav class="navbar">
  <a href="/" class="navbar__link navbar__link--active"></a>
  <a href="/" class="navbar__link"></a>
  <a href="/" class="navbar__link"></a>
 </nav>
```

 In this case, `navbar` is the Block, `navbar__link` is an Element that makes no sense outside of the `navbar` component, and `navbar__link--active` is a Modifier that indicates a different state for the `navbar__link` Element.
 
 Since Modifiers are verbose, many opt to use `is-*` flags instead as modifiers.
 
 ```<a href="/" class="navbar__link is-active"></a>```
 
 These must be chained to the Element and never alone however, or there will be scope issues.
 
 ```
 .navbar__link.is-active {
 }
```

 **Good to hear**
  * Alternative solutions to scope issues like CSS-in-JS
 
* **What are the advantages of using CSS preprocessors?**

  CSS preprocessors add useful functionality that native CSS does not have, and generally make CSS neater and more maintainable by enabling DRY (Don't Repeat Yourself) principles. Their terse syntax for nested selectors cuts down on repeated code. They provide variables for consistent theming (however, CSS variables have largely replaced this functionality) and additional tools like color functions (lighten, darken, transparentize, etc), mixins, and loops that make CSS more like a real programming language and gives the developer more power to generate complex CSS.

  **Good to hear**
  * They allow us to write more maintainable and scalable CSS
  * Some disadvantages of using CSS preprocessors (setup, re-compilation time can be slow etc.)
  
 * **Can you name the four types of @media properties?**
  
   * `all`, which applies to all media type devices
   * `print`, which only applies to printers
   * `screen`, which only applies to screens (desktops, tablets, mobile etc.)
   * `speech`, which only applies to screenreaders

* **What is the difference between em and rem units?**

  Both `em` and `rem` units are based on the `font-size` CSS property. The only difference is where they inherit their values from.
  
  * `em` units inherit their value from the `font-size` of the parent element
  * `rem` units inherit their value from the `font-size` of the root element (`html`)

  In most browsers, the `font-size` of the root element is set to `16px` by default.
  
  **Good to hear**
  * Benefits of using em and rem units
  
 * **Describe the layout of the CSS Box Model and briefly describe each component.**
    
    `Content`: The inner-most part of the box filled with content, such as text, an image, or video player. It has the dimensions `content-box width` and `content-box height`.

    `Padding`: The transparent area surrounding the content. It has dimensions `padding-box width` and `padding-box height`.

    `Border`: The area surrounding the padding (if any) and content. It has dimensions `border-box width` and `border-box height`.

    `Margin`: The transparent outer-most layer that surrounds the border. It separates the element from other elements in the DOM. It has dimensions `margin-box width` and `margin-box height`.
    
    **Good to hear**
    * This is a very common question asked during front-end interviews and while it may seem easy, it is critical you know it well!
    * Shows a solid understanding of spacing and the DOM
 
* **What is the difference between em and rem units?**

  Both `em` and `rem` units are based on the `font-size` CSS property. The only difference is where they inherit their values from.

  * `em` units inherit their value from the `font-size` of the parent element
  * `rem` units inherit their value from the `font-size` of the root element (`html`)
  
  In most browsers, the `font-size` of the root element is set to `16px` by default.

  **Good to hear**
  * Benefits of using em and rem units
  
* **What are the advantages of using CSS sprites and how are they utilized?**

  CSS sprites combine multiple images into one image, limiting the number of HTTP requests a browser has to make, thus improving load times. Even under the new HTTP/2 protocol, this remains true.

  Under HTTP/1.1, at most one request is allowed per TCP connection. With HTTP/1.1, modern browsers open multiple parallel connections (between 2 to 8) but it is limited. With HTTP/2, all requests between the browser and the server are multiplexed on a single TCP connection. This means the cost of opening and closing multiple connections is mitigated, resulting in a better usage of the TCP connection and limits the impact of latency between the client and server. It could then become possible to load tens of images in parallel on the same TCP connection.

  However, according to benchmark results, although HTTP/2 offers 50% improvement over HTTP/1.1, in most cases the sprite set is still faster to load than individual images.

  To utilize a spritesheet in CSS, one would use certain properties, such as `background-image`, `background-position` and `background-size` to ultimately alter the `background` of an element.

  **Good to hear**
  * `background-image`, `background-position` and `background-size` can be used to utilize a spritesheet.
  
* **What is the difference between '+' and '~' sibling selectors?.**

   The General Sibling Selector `~` selects all elements that are siblings of a specified element.

   The following example selects all `<p>` elements that are siblings of `<div>` elements:
   
   ```
   div ~ p {
    background-color: blue;
   }
   ```
  
  The Adjacent Sibling Selector `+` selects all elements that are the adjacent siblings of a specified element.

  The following example will select all `<p>` elements that are placed immediately after `<div>` elements:
  
  ```
  div + p {
   background-color: red;
  }
  ```
  
* **Can you describe how CSS specificity works?**

  Assuming the browser has already determined the set of rules for an element, each rule is assigned a matrix of values, which correspond to the following from highest to lowest specificity:

  * Inline rules (binary - 1 or 0)
  * Number of id selectors
  * Number of class, pseudo-class and attribute selectors
  * Number of tags and pseudo-element selectors
  
  When two selectors are compared, the comparison is made on a per-column basis (e.g. an id selector will always be higher than any amount of class selectors, as ids have higher specificity than classes). In cases of equal specificity between multiple rules, the rules that comes last in the page's style sheet is deemed more specific and therefore applied to the element.

  **Good to hear**
  * Specificity matrix: [inline, id, class/pseudo-class/attribute, tag/pseudo-element]
  * In cases of equal specificity, last rule is applied
  
* **What is a focus ring? What is the correct solution to handle them?**

   A focus ring is a visible outline given to focusable elements such as buttons and anchor tags. It varies depending on the vendor, but generally it appears as a blue outline around the element to indicate it is currently focused.

  In the past, many people specified `outline: 0`; on the element to remove the focus ring. However, this causes accessibility issues for keyboard users because the focus state may not be clear. When not specified though, it causes an unappealing blue ring to appear around an element.

  In recent times, frameworks like Bootstrap have opted to use a more appealing `box-shadow` outline to replace the default focus ring. However, this is still not ideal for mouse users.

  The best solution is an upcoming pseudo-selector `:focus-visible` which can be polyfilled today with JavaScript. It will only show a focus ring if the user is using a keyboard and leave it hidden for mouse users. This keeps both aesthetics for mouse use and accessibility for keyboard use.
  
* **What is CSS selector specificity and how does it work?**
* **What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?**
* **Describe Floats and how they work.**
* **Describe `z-index` and how stacking context is formed.**
* **Describe BFC (Block Formatting Context) and how it works.**
* **What are the various clearing techniques and which is appropriate for what context?**
* **How would you approach fixing browser-specific styling issues?**
* **How do you serve your pages for feature-constrained browsers?**
* **What techniques/processes do you use?**
* **What are the different ways to visually hide content (and make it available only for screen readers)?**
* **Have you ever used a grid system, and if so, what do you prefer?**
* **Have you used or implemented media queries or mobile specific layouts/CSS?**
* **Are you familiar with styling SVG?**
    Yes, there are several ways to color shapes (including specifying attributes on the object) using inline CSS, an embedded CSS section, or an external CSS file. Most SVG you'll find around the web use inline CSS, but there are advantages and disadvantages associated with each type.

   Basic coloring can be done by setting two attributes on the node: `fill` and `stroke`. `fill` sets the color inside the object and `stroke` sets the color of the line drawn around the object. You can use the same CSS color naming schemes that you use in HTML, whether that's color names (that is `red`), RGB values (that is `rgb(255,0,0)`), Hex values, RGBA values, etc.
   ```
   <rect x="10" y="10" width="100" height="100" stroke="blue"
   fill="purple" fill-opacity="0.5" stroke-opacity="0.8"/>
   ```
   The above `fill="purple"` is an example of a presentational attribute. Interestingly, and unlike inline styles like `style="fill: purple"` which also happens to be an attribute, presentational attributes can be overriden by CSS styles defined in a stylesheet. So, if you did something like `svg { fill: blue; }` it would override the purple fill we've defined.
* **Can you give an example of an `@media` property other than `screen`?**
* **What are some of the "gotchas" for writing efficient CSS?**
    Firstly, understand that browsers match selectors from rightmost (key selector) to left. Browsers filter out elements in the DOM according to the key selector and traverse up its parent elements to determine matches. The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector. Hence avoid key selectors that are tag and universal selectors. They match a large number of elements and browsers will have to do more work in determining if the parents do match.

    BEM (Block Element Modifier) methodology recommends that everything has a single class, and, where you need hierarchy, that gets baked into the name of the class as well, this naturally makes the selector efficient and easy to override.

    Be aware of which CSS properties trigger reflow, repaint, and compositing. Avoid writing styles that change the layout (trigger reflow) where possible.
* **What are the advantages/disadvantages of using CSS preprocessors?**
* **Describe what you like and dislike about the CSS preprocessors you have used.**
* **How would you implement a web design comp that uses non-standard fonts?**
* **Explain how a browser determines what elements match a CSS selector.**
* **Describe pseudo-elements and discuss what they are used for.**
* **Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.**
* **What does `* { box-sizing: border-box; }` do? What are its advantages?**
* **What is the CSS `display` property and can you give a few examples of its use?**
* **What's the difference between `inline` and `inline-block`?**
I shall throw in a comparison with `block` for good measure.

|                                      | `block`                                                                                     | `inline-block`                                                      | `inline`                                                                                                                                                                                                             |
| ------------------------------------ | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Size                                 | Fills up the width of its parent container.                                                 | Depends on content.                                                 | Depends on content.                                                                                                                                                                                                  |
| Positioning                          | Start on a new line and tolerates no HTML elements next to it (except when you add `float`) | Flows along with other content and allows other elements beside it. | Flows along with other content and allows other elements beside it.                                                                                                                                                  |
| Can specify `width` and `height`     | Yes                                                                                         | Yes                                                                 | No. Will ignore if being set.                                                                                                                                                                                        |
| Can be aligned with `vertical-align` | No                                                                                          | Yes                                                                 | Yes                                                                                                                                                                                                                  |
| Margins and paddings                 | All sides respected.                                                                        | All sides respected.                                                | Only horizontal sides respected. Vertical sides, if specified, do not affect layout. Vertical space it takes up depends on `line-height`, even though the `border` and `padding` appear visually around the content. |
| Float                                | -                                                                                           | -                                                                   | Becomes like a `block` element where you can set vertical margins and paddings.                                                                                                                                      |
* **What's the difference between the `nth-of-type()` and `nth-child()` selectors?**
* **What's the difference between a `relative`, `fixed`, `absolute` and statically positioned element?**
* **What existing CSS frameworks have you used locally, or in production? How would you change/improve them?**
* **Have you used CSS Grid?**
* **Can you explain the difference between coding a web site to be responsive versus using a mobile-first strategy?**
* **Have you ever worked with retina graphics? If so, when and what techniques did you use?**
* **Is there any reason you'd want to use `translate()` instead of absolute positioning, or vice-versa? And why?**
