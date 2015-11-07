## JavaScript Style Guide

----

* [Syntax](#syntax)
  * [Naming](#naming)
  * [Naming private methods and properties](#naming-private-methods-and-properties)
  * [File names](#file-names)
  * [Indentation](#indentation)
  * [Braces](#braces)
  * [Line length](#line-length)
  * [require() lines.](#require-lines)
* [Comments and documentation](#comments-and-documentation)
  * [Inline Comments](#inline-comments)
  * [Top level file and class comments](#top-level-file-and-class-comments)
  * [Methods and properties comments](#methods-and-properties-comments)
* [Core language rules](#core-language-rules)
  * [Equality](#equality)
  * [Array and Object literals](#array-and-object-literals)
  * [Use a new var statement for each declaration](#use-a-new-var-statement-for-each-declaration)
  * [Avoid href="#" for JavaScript triggers](#avoid-href-for-javascript-triggers)
  * [Use modules, not global variables](#use-modules-not-global-variables)
* [ES6/7 rules](#es67-rules)
  * [Use =&gt; instead of bind(this) ](#use--instead-of-bindthis)
  * [Use backticks for string interpolation](#use-backticks-for-string-interpolation)
  * [Do not use ES6 classes for React classes](#do-not-use-es6-classes-for-react-classes)
  * [Do not use async/await or generators](#do-not-use-asyncawait-or-generators)
  * [Do not use Set or Map ](#do-not-use-set-or-map)
  * [Use let and const for new files; do not use var ](#use-let-and-const-for-new-files-do-not-use-var)
* [Library rules](#library-rules)
  * [Use $ for jQuery](#use--for-jquery)
  * [Don't use Underscore](#dont-use-underscore)

----

This guide is adapted from the jQuery style guide.

----------
### Syntax

#### Naming

```js
ClassNamesLikeThis
methodNamesLikeThis
variableNamesLikeThis
parameterNamesLikeThis
propertyNamesLikeThis
SYMBOLIC_CONSTANTS_LIKE_THIS
```

When naming variables and properties referring to jQuery element
objects, prefix the name with `$`:

```js
function doSomethingFancy(selector) {
  var $elements = $(selector);
  ...
}
```

#### Naming private methods and properties

Private methods and properties (in files, classes, and namespaces)
should be named with a leading underscore.

While we do not currently use any compilers to enforce this, clients
of an API or class are expected to respect these conventions.

```js
function _PrivateClass() {
    // should not be instantiated outside of this file
}

function PublicClass(param) {
    this.publicMember = param;
    this._privateMember = new _PrivateClass();
}

var x = new _PrivateClass();  // OK - we’re in the same file.
var y = new PublicClass();    // OK
var z = y._privateMember;     // NOT OK!
```

Rationale: leading underscores for private methods and properties is
consistent with the styles used in numerous JavaScript libraries, many
of which we include in our code base (e.g. Backbone). It is also
consistent with our Python style guide, lowering the mental effort for
developers to switch between the two.

#### File names

```
file-names-like-this.js
template-names-like-this.handlebars
```

#### Indentation

Use 2-space indenting for all code. Do not use tabs.

Extra indentation should be used to clearly distinguish multiline
conditionals from the following block of code.

No:
```js
if (someReallyLongBooleanVariableIMeanReallyLong &&
  someOtherBoolean) {
  return "monkeys";
}
```

Yes:
```js
if (someReallyLongBooleanVariableIMeanReallyLong &&
  someOtherBoolean) {
  return "monkeys";
}
```

#### Braces

Braces should be preferred on blocks unless overly verbose. Good places to not use braces are things like guards.

`if/else/for/while/try` should have braces and always go on
multiple lines, with the opening brace on the same line.

No:
```js
if (true)
  blah();
else
  bleh();
```

Yes:
```js
if (true) {
  blah();
} else {
  bleh();
}
```

Ok:
```js
function foo() {
  if (x) return true;
  if (y) return false;
  // do other
}
```

`else/else if/catch` should go on the same line as the brace:

```js
if (blah) {
  baz();
} else {
  baz2();
}
```

#### Line length

Lines should not exceed 120 characters.  It's 2015 and we're not coding on punch cards and terminals anymore.


#### `require()` lines.

Separate first party and third party `require()` lines, and sort
`require()` lines.

"First party" code is anything we wrote whose primary source lives in
the repository its being used in.  Lodash is third party because
we didn't write it.  Patternity is third party in hub because even
though we wrote it, its primary sources lives in a different
repository.

Imports should be sorted lexicographically (as per unix `sort`).

No:
```js
var _ = require("underscore");
var $ = require("jquery");
var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
var HappySurvey = require("../missions-package/happy-survey.jsx");
var DashboardActions = require('./datastores/dashboard-actions.js');
var React = require("react");
var UserMission = require("../missions-package/user-mission.js");
var Kicksend = require("../../third_party/javascript-khansrc/mailcheck/mailcheck.js");
```

Yes:
```js
var $ = require("jquery");
var Kicksend = require("../../third_party/javascript-khansrc/mailcheck/mailcheck.js");
var React = require("react");
var _ = require("underscore");

var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var DashboardActions = require('./datastores/dashboard-actions.js');
var HappySurvey = require("../missions-package/happy-survey.jsx");
var UserMission = require("../missions-package/user-mission.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
```

Object destructuring should go after all require lines.

Write requires on a single line, even if they extend past 120 chars, so they are easier to sort. The linter will ignore these lines.

No:
```js
var React = require("react");
var ReactART = require("react-art");
var Group = ReactART.Group;
var Path = ReactART.Path;
var _ = require("underscore");

var ItemStore = require("./item-store.jsx");
```

Yes:
```js
var React = require("react");
var ReactART = require("react-art");
var _ = require("underscore");

var ItemStore = require("./item-store.jsx");

var Group = ReactART.Group;
var Path = ReactART.Path;
```


------------------------------
### Comments and documentation

#### Inline Comments

Inline style comments should be of the `//` variety, not the `/* */`
variety.

#### Top level file and class comments

All files and classes should have JSDoc comments.

JSDoc can be parsed by a number of open source tools, and must be well-formed. 

Syntax:
```js
/**
 * A JSDoc comment should begin with a slash and 2 asterisks.
 */
```

Top-level (top-of-file) comments are designed to orient readers
unfamiliar with the code to what is in this file and any other
disclaimers clients of the code should be given.  It should provide a
description of the file's contents and any dependencies or
compatibility information.  As an example:

```js
/**
 * Various components to handle management of lists of coaches for
 * the profile page.
 *
 * These utilities were not written to be a general purpose utility
 * for the entire code base, but has been optimized with the 
 * assumption that the Profile namespace is fully loaded.

 */
```

Class comments should be used for every class, and give a description
along with appropriate type tags (see "Methods and properties"
comments for more information on types on the constructor).

```js
/**
 * Class making something fun and easy.
 *
 * @param {string} arg1 An argument that makes this more interesting.
 * @param {Array.<number>} arg2 List of numbers to be processed.
 */
function SomeFunClass(arg1, arg2) {
  // ...
}
```

#### Methods and properties comments

All non-trivial methods and properties should also have JSDoc comments.

Type annotations are strongly encouraged; if there is even a slight
chance that the type will be ambiguous to future readers, put in a
type annotation.

Type annotations are based on the ES4/JS2 type system, and are
documented in the [Google JavaScript style
guide](https://google.github.io/styleguide/javascriptguide.xml).

`@param` and `@return` type annotations that have comments that do not
fit on one line wrap to the next line and indent 4 spaces.

Example:

```js
/**
 * A UI component allows users to select badges from their full list
 * of earned badges, displaying them in a container.
 * Expects a Badges.BadgeList as a model.
 */
Badges.DisplayCase = Backbone.View.extend({
    /**
     * Whether or not this is currently in edit mode and the full
     * badge list is visible.
     */
    editing: false,

    /**
     * The full user badge list available to pick from when in edit mode.
     * @type {Badges.UserBadgeList}
     */
    fullBadgeList: null,

    /**
     * Enters "edit mode" where badges can be added/removed.
     * @param {number=} index Optional index of the slot in the display-case
     *     to be edited. Defaults to the first available slot, or if none
     *     are available, the last used slot.
     * @return {Badges.DisplayCase} This same instance so calls can be
     *     chained.
     */
    edit: function(index) {
    …
    },
   ...
};
```

-----------------------
### Core language rules

#### Equality

Prefer `===` (strict equality) to `==` due to the [numerous oddities
related to JavaScript's type coercion](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/).

The only valid use of `==` is for comparing against null and undefined
at the same time:

```js
// Check null and undefined, but distinguish between other falsey values
if (someVariable == null) {
```

Though you will often want to just check against falsey values, and
can just say `if (!someVariable) { ... }`.

#### Array and Object literals

Always use `[]` and `{}` style literals to initialize arrays and
objects, not the `Array` and `Object` constructors.

Array constructors are error-prone due to their arguments: `new
Array(3)` yields `[undefined, undefined, undefined]`, not `[3]`.

To avoid these kinds of weird cases, always use the more readable
array literal.

Object constructors don't have the same problems, but follow the same
rule for consistency with arrays.  Plus, `{}` is more readable.

#### Use a new var statement for each declaration

No:
```js
var a = "foo",
  b = a + "bar",
  c = fn(a, b);
```

Yes:
```js
var a = "foo";
var b = a + "bar";
var c = fn(a, b);
```

A single var statement is bad because:

* If you forget a comma, you just made a global 
* It originated when people wanted to save bytes, but we have a minifier
* It makes line-based diffs/editing messier
* It encourages C89-style declarations at the top of scope, preventing
  you from only declaring vars before first use, the latter preferable
  as it conveys intended scope to the reader

#### Avoid `href="#"` for JavaScript triggers

When you want a link-like thing rather than a button to trigger a
JavaScript operation, rather than going to a new address.

Here's a discussion on Stack Overflow about options:
http://stackoverflow.com/questions/134845/href-tag-for-javascript-links-or-javascriptvoid0


No:
```js
<a href="#">Flag</a>
```

Yes:
```js
<a href="javascript:void 0">Flag</a>
```

#### Use modules, not global variables

We use commonjs and es6 transpiled to commonjs modules in all our apps.

There are mechanisms for an explicit import/export mechanism rather than 
using global variables to export functionality.

No:
```js
window.Jungle = {
    welcome: function() {
        // ...
    },
    haveFever: function() {
        // ...
    }
};
```

**NO**:
```js
window.welcome = function() {
   // ...
};

window.haveFever = function() {
   // ...
};
```

Yes:
```js
var Jungle = {
    welcome: function() {
        // ...
    },
    haveFever: function() {
        // ...
    }
};

module.exports = Jungle;
```

Yes:
```js
export function welcome() {
  // ... 
}

export function haveFever() {
  // ...
}

export default function Jungle() {
  // ...
}
```

You can export multiple objects in one file, but consider if it
wouldn't be better to split up the file to maintain one export per file.

---------------
### ES6/7 rules

Several of our supported browsers support only ES5 natively.  We use
babel to emulate some -- but not all -- ES6 and ES7 language
features.

In some cases, we do not yet allow a new language feature, if it's
expensive to polyfill/transpile.  In others, we require using the newer language
feature and avoiding the old:

| Construct | Use...                                | ...instead of |
| --------- | ------------------------------------- | ---------------------- |
| backticks | `` `http://${host}/${path}` `` | `"http://" + host + "/" + path` |
| destructuring | `var { x, y } = a;` | `var x = a.x; var y = a.y;` |
| fat arrow | `foo(() => { ... })` | `foo(function() { ... }.bind(this))` |
| let/const | `let a = 1; const b = "4EVAH"; a++;` | `var a = 1; var b = "4EVAH"; a++;` |
| includes | `array.includes(item)` | `array.indexOf(item) !== -1` |
| for/of | `for (const [key, value] of Object.entries(obj)) { ... }` | `_.each(obj, function(value, key) { ... })` |
| spread | `{ ...a, ...b, c: d }` | `_.extend({}, a, b, { c: d })` |
| rest params | `function(bar, ...args) { foo(...args); }` | `function(bar) { var args = Array.prototype.slice.call(arguments, 1); foo.apply(null, args); }` |

#### Use `=>` instead of `bind(this)`

Arrow functions are easier to read (and with Babel, more efficient)
than calling `bind` manually.

#### Use rest params instead of `arguments`

The magic `arguments` variable has some odd quirks. It's simpler to
use rest params like `(...args) => foo(args)`.

#### Use backticks for string interpolation

`+` is not forbidden, but backticks are encouraged!

#### Use ES6 classes for React classes

Prefer Higher Order components over mixins.

#### Do not use `async`/`await` or generators

This is because the polyfill for these constructs generates very large
code.

This rule may change once all our supported browsers support ES6
natively.

#### Do not use `Set` or `Map`

The polyfills for these, though not huge, are large enough it's not
worth the (small) benefit of using these classes for hashtables
instead of just using `object`.

This rule may change if strong enough support for these types is
evinced.

#### Use `let` and `const` for new files; do not use `var`

`let` is superior to `var`, so prefer it for new code.

This rule will become mandatory everywhere once we have done a fixit
to replace all uses of `var` in existing files.


-----------------
### Library rules

#### Use `$` for jQuery

We use `$` as the jQuery identifier, as opposed to typing out `jQuery`
in full.

No:
```js
jQuery(".some-class span").hide();
```

Yes:
```js
$(".some-class span").hide();
```

#### Use Lodash when it makes sense.

When doing simple operations es5/es6 built ins should be preferred in order to keep bundle size down.

However when doing complex operations or working on scripts/serverside code lodash can enable you to write much more performant, readable, and reusable code.

