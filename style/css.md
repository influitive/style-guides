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

The default configuration makes it so autoprefixer runs on all our css as well
as allowing nesting. The config for postcss can be found in the root of all projects.

## CSS modules

We are also using CSS modules instead of global CSS, allowing us to scope CSS to the react component. This allows us to write css code that doesn't care about the css anywhere else.

generally what this looks like is this:

styles.css
```css
.card {
  background-color: red;
}
```

component.js
```js
import styles from './styles.css';
...
export default ({prop1, prop2}) => (
  <div className={styles.card}>
  </div>
)
```

This example will apply the classname 'card' to the div. However, when you look at the html css, the class will actually look like a garbled mess. This means that if someone else's css uses the class card, they will get a different card class.

### Composing CSS

There are certainly cases when you want to use the exact same class as another component and this can be done with the ```compose``` keyword.

styles1.css
```css
.card {
  background-color: red;
}
```

styles2.css
```css
.special-card {
  composes: card from "./styles.css";
}
```

This essentially allows you to import classes from different files.

### Container Queries

Container queries are like media queries but better. Instead of just considering the viewport, container queries look at the container of a component.

This feature is still in flux but we are using a library to get some basic benefits. https://github.com/d6u/react-container-query

```js
import wrap from '../../utils/cq-wrap';

const c1 = ({ ...}) => (
  ...
);

const query = {
  width_between_400_and_599: {
    minWidth: 400,
    maxWidth: 599
  },
  width_larger_than_500: {
    minWidth: 500
  }
};

export default wrap(c1, query);
```

This setup returns a component that has some custom attributes set based on it's container's width. In this case, the attribute ```width_between_400_and_599``` will be applied when the container is between 400px and 599px. ```width_larger_than_500``` when it's width is over 500px. Each query object can only look at (min|max)(width|height)

The css to use these attributes is:

```css
.card-list[width_between_400_and_599] {
  background-color: red;
}

.card-list[width_larger_than_500] {
  color: blue;
}
```

The attributes can actually be named anything but it's good practice to have the attributes just explain exactly what they are checking for.

Also, given the nature of how container queries work, be careful if you're css properties would change the width/height of the container, which can cause an infinite loop of renders.
