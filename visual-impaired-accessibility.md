### ARIA

- [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

### aria-label vs aria-labelledby

- [ARIA: aria-label attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label)

- [ARIA: aria-labelledby attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)

- **aria-label**

  - Used to directly provide a hidden label text to an element.

  - The string specified in the aria-label attribute becomes the element's Accessible Name. Screen readers read the value of this attribute to inform users of the element's purpose.

  - aria-label applies only to the element itself. It cannot reference the text of other elements.

  - If used together with aria-labelledby or a `<label>` tag, aria-label takes the highest priority. In other words, if aria-label is specified, other labeling methods are ignored.

  - In short, use this when there is no visible text label at all (such as an icon-only button), or when the existing text is ambiguous and you want to provide a short, clear alternative text.

  - Examples:

    - Icon button without a text label: For example, when you want to give a "Search" label to a search button that only has a magnifying glass icon and no text.

    - When visible text is insufficient: If a "More" button is unclear about what it refers to, you can provide a more specific label like "More news".

    - When there is text inside the element, but it serves another purpose: For example, `<button>X</button>` where "X" is text, but the button is actually for "Close", so you use aria-label="Close".

    ```html
    <button aria-label="Search">
      <img src="magnifying-glass.png" alt="Magnifying glass icon" />
    </button>

    <div role="navigation" aria-label="Main menu"></div>
    ```

- **aria-labelledby**

  - Used to provide an Accessible Name for an element by referencing the IDs of other elements on the page.

  - The text content of one or more elements specified by the aria-labelledby attribute (by their IDs) is concatenated in order to form the label for the current element.

  - Examples:

    - When you need to combine the text of multiple elements to create a label: For example, when a table cell's meaning is clear only when combining several header contents.

    - When you want to use already existing visible text: If there is already a visible text label in the HTML document, but you cannot use a `<label>` tag due to a complex structure, you can reference the ID of the text element.

    - Complex widgets: For widgets like modal dialogs or tab panels where the title exists as a separate element and you want to use that title as the widget's label.

    ```html
    <h2 id="dialog-title">Review Cart</h2>

    <div role="dialog" aria-labelledby="dialog-title">
      <p>Please review your selected items.</p>
      <button>Confirm</button>
    </div>

    <div id="product-name">Laptop Pro Max</div>
    <button aria-labelledby="product-name">View Details</button>
    ```

  - In summary, use this when there is already text on the page that can serve as a label, when you need to combine multiple pieces of text, or when you want to associate an existing heading as the label for a complex widget.

- [ARIA: aria-live attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live)

- [ARIA: aria-atomic attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic)

### color contrast

- [Color contrast](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast)

- [Chrome - Rendering tab overview](https://developer.chrome.com/docs/devtools/rendering)

--

### Reference

- [The Only Accessibility Video You Will Ever Need](https://www.youtube.com/watch?v=2oiBKSjOOFE&t=1071s)
