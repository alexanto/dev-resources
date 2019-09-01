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
