# CSS

Welcome to the CSS style guide. This guide borrows heavily from Github's [Primer CSS guidelines](http://primercss.io/guidelines/), so thanks to them!

## Coding Style

- Use a four space indent.
- Put spaces after : in property declarations.
- Spaces before braces: put spaces before { in rule declarations.
- Do not use trailing commas when listing multiple selectors
- Separate selector and property declarations with a new line
- Separate rules with a new line
- End properties declarations with a ;
- Alphabetize properties within rule declarations
- Properties with values of 0 should *not* have units
- Use lowercase hex color codes #fff unless using rgba.
- Use /* */ for block comments (instead of //).
- Use a maximum line length of 80 characters (rationale: looking at files side-by-side)
- Use dashes in selectors instead of underscores (.my-class, not .my_class)

Here is good example syntax:

```css
/* This is a good example! */
.styleguide-format,
.other-format,
.third-format {
    background: rgba(0,0,0,0.5);
    border: 1px solid #0f0;
    color: #000;
    margin: 0;
}

.next-rule {
    color: #000;
}
```

## CSS Specificity Guidelines

Elements that occur **exactly once** inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

- **Good** candidates for ids: header, footer, modal popups.
- **Bad** candidates for ids: navigation, item listings, item view pages (ex: issue view).

- If you must use an id selector (`#selector`) make sure that you have no more than one in your rule declaration. A rule like `#header .search #quicksearch { ... }` is considered harmful. Curious why? Check out this primer on [CSS specificity](http://css-tricks.com/specifics-on-css-specificity/).
- Do not combine id selectors with tags (`div#container`). The id should be specific enough, and should not be repeated elsewhere to necessitate the tag name. Try to follow the same guidelines for classes as well.
- When modifying an existing element for a specific use, try to use specific class names. Instead of `.listings-layout.bigger` use rules like `.listings-layout.listings-bigger`. Think about ack/greping your code in the future.
- Key (rightmost) Selectors should be as specific as possible. For example `a.navigation-link` instead of `#navigation-links a`. This has [performance implications](http://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/).

## Post CSS

We are using Post CSS processors instead of LESS. This allows us to pick and choose which operators
we want to use on our css.

## CSS modules

We are also using CSS modules instead of global CSS, allowing us to scope CSS to the react component.
