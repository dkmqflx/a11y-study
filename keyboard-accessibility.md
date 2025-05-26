## tabindex

- The tabindex attribute is used to control the keyboard navigation order (focus order).

- tabindex="0": Include in the natural flow

  - tabindex="0" is used when you want to include an HTML element that is not normally focusable (e.g., `<div>`, `<span>`, or `<a>` without an href) in the natural tab order (document order).

- Make non-semantic elements focusable: When you add interactive behavior (e.g., click event) to elements like div or span that are not natively focusable, you should make them accessible by keyboard as well.

```html
<div tabindex="0" role="button" aria-label="Open menu">
  <img src="menu-icon.png" alt="Menu icon" />
</div>
```

- Improve screen reader accessibility: Use tabindex to allow screen reader users to access and read content that was not previously focusable.

- It's important to use ARIA attributes like role or aria-label together to convey the element's meaning.

- Maintain natural order: Elements with tabindex="0" are included in the tab order according to their position in the HTML document. That is, they are navigated in the same order as other focusable elements (buttons, links, input fields, etc.).

```html
<button>Submit</button>

<div tabindex="0" role="button" aria-label="Close dialog">X</div>
<input type="text" placeholder="Enter name" />
```

### tabindex with overflow: scroll

- Why you need `tabindex="0"` on elements with `overflow: scroll`:

  - Containers like `<div>` with `overflow: scroll` or `overflow: auto` create scrollbars when content overflows. Mouse users can easily scroll by clicking or dragging the scrollbar.

  - However, keyboard users and screen reader users may encounter problems.

- **Keyboard users**: By default, div elements are not focusable by keyboard. To use arrow keys (Up, Down, Page Up, Page Down, etc.) to scroll within the area, the user must first be able to focus the scrollable area. Without tabindex="0", keyboard users cannot focus the scroll area and cannot scroll it.

- **Screen reader users**: Screen readers navigate focusable elements and read their content. If a scrollable area is not focusable (like a div), the screen reader may treat it as a single block or not recognize its content at all. Adding tabindex="0" allows screen readers to stop at the area and explore its content.

- Example:

```html
<style>
  .scrollable-area {
    width: 300px;
    height: 200px;
    border: 1px solid #ccc;
    overflow: auto; /* or overflow: scroll */
    padding: 10px;
  }
  .long-content {
    height: 500px; /* Long content to trigger scrolling */
    background-color: #f0f0f0;
  }
</style>

<h2>Scrollable Area</h2>

<!-- 
  tabindex="0": Includes the `div` in the keyboard tab order so users can focus it with the Tab key.
  role="region": Announces the `div` as a main section to screen readers.
  aria-label="Long document content": Describes the purpose or content of the scrollable area to screen reader users.
 -->
<div
  class="scrollable-area"
  tabindex="0"
  role="region"
  aria-label="Long document content"
>
  <div class="long-content">
    <p>This is the content of the scrollable area.</p>
    <p>Keyboard users can press Tab to focus this area,</p>
    <p>and use the arrow keys (up/down) or Page Up/Down to scroll.</p>
    <p>
      Screen reader users can also access and explore the content in this area.
    </p>
    <p>The content continues...</p>
    <p>.... (more content)</p>
  </div>
</div>
<button>Next item</button>
```

## outline:none with focus-visible

- [2.4.7 Focus Visible (Level AA)](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html)

- If you use outline: none on buttons or other interactive elements, you must provide a visible focus indicator for keyboard accessibility.

- Outline is the browser's default focus indicator. When users navigate with the Tab key, the currently focused element is outlined, making it visually clear which element is active.

- However, using `outline: none` removes this important visual indicator, which causes the following problems:

  - Keyboard users: Users who navigate with the Tab key (especially those who cannot use a mouse or prefer keyboard for productivity) will not know which element is focused, causing severe confusion. It's like navigating a web page blindfolded.

  - Screen reader users: Screen readers announce the focused element, but without a visual indicator, it's hard for users to describe the location to others or relate it to other visual information.

  - Accessibility violation: WCAG (Web Content Accessibility Guidelines) emphasizes that all UI components must have a clear keyboard focus indicator as part of the "Keyboard Accessible" principle. Using outline: none is likely to violate this guideline.

- You can solve these problems using focus-visible.

- `:focus-visible` is a CSS pseudo-class. It applies styles only when the user agent (browser) heuristically determines that a visual focus indicator should be shown.

- Works only for keyboard focus: Styles are applied only when the user focuses an element using keyboard navigation (Tab, arrow keys, etc.).

- Not applied on mouse click: If the user clicks an element with the mouse, `:focus-visible` styles are not applied.

```css
/* Remove default outline */
button {
  outline: none;
}

/* Style applied only when focused via keyboard */
button:focus-visible {
  border: 2px solid blue; /* Blue outline */
}
```

### :focus vs :focus-visible comparison

- `:focus` (reacts to all focus events)

  - How it works: Styles are applied whenever an element receives focus.
  - Examples:

    - Focused by keyboard (Tab, arrow keys) -> Style applied

    - Focused by mouse click -> Style applied

    - Focused by script (element.focus()) -> Style applied

  - Problem: Focus styles (like outline) may appear unnecessarily in situations where the user already knows which element is focused (e.g., mouse click), which can be visually distracting.

  - This is why outline: none became popular.

- `:focus-visible` (applies selectively based on browser heuristics)

  - How it works: Styles are applied only when the browser determines that a focus indicator is useful for the user.
  - Browser heuristics examples:

    - Keyboard navigation: User presses Tab to move to the next element (keyboard users need a focus indicator) -> Style applied

    - Scripted focus (sometimes): JavaScript moves focus to an element as the next step in user input -> Style applied

    - Mouse click: User clicks a button to focus it (user already knows where focus is) -> Style not applied

    - Touch: User taps an element on a touchscreen -> Style not applied (similar to mouse)

  - Purpose: Even if you use outline: none, :focus-visible ensures that keyboard users (and those who need a focus indicator) still get a focus style, while hiding it in unnecessary situations for a better visual experience.

## Invisible Unclickable Link

- [2.4.1 - Bypass Blocks (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/bypass-blocks.html)

- A "skip to" link can significantly improve website accessibility, especially for users navigating with keyboards or screen readers.

- It allows them to bypass navigation menus and directly access the specific content.

```html
<body>
  <!-- Place an invisible link at the top of the page that points to the main content area using an anchor tag (<a>) and an href attribute that matches the id of your main content element -->
  <a href="#main-content" class="skip-link">Skip to main content</a>
  <a href="#footer-content" class="skip-link">Skip to main footer</a>

  <header>
    <nav>
      <a href="#">Link 1</a>
      <a href="#">Link 2</a>
    </nav>
  </header>

  <main id="main-content">
    <h1>Welcome to the Main Content</h1>
    <p>This is the primary content of the page.</p>
  </main>

  <footer id="footer-content">
    <li>footer</li>
  </footer>
</body>
```

```css
.skip-link {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #333;
  color: white;
  text-align: center;
  padding: 0.5rem;
  z-index: 100; /* Ensure it's on top of other elements */
  translate: 0 -100%;
  transition: translate 150ms ease-in-out;
}

.skip-link:focus {
  translate: 0;
}

/* Optional: Style for other elements when not focused on skip link */
*:focus:not(.skip-link) {
  outline: 2px solid yellow; /* Example focus style */
}
```

---

### Reference

- [MDN - tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/tabindex)

- [MDN - :focus vs :focus-visible](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible#focus_vs_focus-visible)

- [The Only Accessibility Video You Will Ever Need](https://www.youtube.com/watch?v=2oiBKSjOOFE&t=1071s)

- [I Love This CSS Focus Hack](https://www.youtube.com/watch?v=dAOGdIOcLps)

- [Why Does Nearly Every Site Have This "Invisible Unclickable" Link?](https://www.youtube.com/watch?v=VUR0I5mqq7I)

- [HTML tabindex attribute and keyboard focus](https://www.daleseo.com/html-tabindex/)
