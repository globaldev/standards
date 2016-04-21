# CSS

## Style Guide

### Whitespace

* All source files use spaces, not tabs.
* The indent level is always 2 spaces.
* Feel free to use blank lines to make your code easier to read (they'll be
  removed by the minifier anyway).
* Don't leave trailing white space at the end of any line.

### Syntax

For all new components we will be looking to use an OOCSS methodology based on
the BEM syntax as [explained well][bem-syntax] by Harry Roberts.

[bem-syntax]: http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/

#### Rules & Blocks

* Avoid using #ID's or basic elements (`h2`, `p`, `span`) as selectors. They
  usually end up either too specific and not open to intended modifiers or not
  dynamic enough to handle other semantic constructs.
* No element tags should be included in any `.scss` file except when assigning
  reset/normalised styles. Exceptions can be made in the case of `h1-h6`,
  providing they have previously been reset to base font-size, margin, padding
  and line-height.

```scss
// This is BAD practice
h1 { 
  font-size: 200%;
  line-height: 1.75;
}
#title {
  font-size: 180%;
}

// This is GOOD
.heading {
  font-size: 200%;
  line-height: 1.75;
}
.title {
  font-size: 180%;
}
````

#### Quotes

* Single quotes should be used and must be used consistently
* Nested quotes can be escaped where necessary

```scss
.selector:before {
  content: '';
  background-image: url('assets/img/logo.png');
}
```

#### Semicolons

This Sass compiler will tell you off for most instances of missing semicolons
but please always append the last rule per block with a semi-colon just like
you would with every other rule.

```scss
.selector {
  property: value;
  property: value;

  .selector {
    property: value;
    property: value;
  }
}
```

#### Naming

* Lowercase naming should be used throughout
* Sass variables should always use underscores to separate words
* Classnames should always use hyphens when separating words
* @function, @mixin and %extension names should be hyphenated when separating
  words

```scss
$theme_color: red;

.online-status {...}

@function contrast-ratio($a, $b) {...}

@mixin visually-hidden {...}

%button-mimic {...}
```

#### Comments

**Component/Module Header**

Depending on whether a comment is to be compiled in the output, comment blocks
can be written in different ways:

* `//` will always only appear in development
* `/* */` and `/** */` will appear in the compiled output

When defining blocks of CSS, we would prefer to use `/* */` so that the output
will still display those:

```scss
/************************************************************
 **  Flyout                                                **
 ************************************************************/
 ```

**Section blocks:**

* When defining descriptive elements and developer notes, we would prefer these
  not be shown in production:

```scss
// Pseudo Elements
// 
// Any Additional notes should be placed here and
// returned when necessary to keep the comments
// developer readable.
// 
// @usage: ...
```

#### Basic Style Declaration

* Each selector should contain an empty line before the first rule is defined
* Each rule attribute should be followed by its colon `:` and a space
* Each rule value should be followed by a semi-colon `;` regardless of whether
  it is the last rule or not

```scss
.selector {
  property: value;
}
```

* Multiple listed selectors should be listed as a stack, a new line per full
  selector

```scss
.selector,
.selector .item,
.selector .item-link {
  property: value;
}
```

Nested rules should be treated as their parent rules, with the only exception
being they are indented appropriately as if they were rules.

```scss
.selector {
  property: value;

  .selector {
    property: value;
  }
}
```

**How Mixins should be included**

* Mixins with more than three arguments/values to be passed should have them
  listed one per line
* Trailing spaces are not essential when including attributes, as only commas
  are needed to separate

```scss
.selector {

  @include mixin(
    value1,
    value2,
    value3,
    value4
  );
}
```

* Mixins with less than three arguments/values to be passed may be listed
  inline (spaces included to improve readability, contradictory to block
  listing as above)

```scss
.selector {
  @include mixin(value1, value2, value3);
}
```

* Mixins with no arguments/values to be passed should be written like any other
  class or attribute rule

```scss
.selector {
  @include mixin();
}
```

#### Colours

* Each element of RGB/A or HSL/A should be separated with a comma and space,
  with no preceding or trailing white space
* Alpha values should be in decimal format, relative to 1.0, not in percentages
* It is advised when using `hsla()` that a fallback, preferably Hex colour
  should be added

```scss
.selector {
  color: #f0f0f0;
  color: rgb(240, 240, 240);
  color: rgba(240, 240, 240, 0.5);

  color: #e2ecf0;
  color: hsl(190, 30%, 94%);
  color: hsla(190, 30%, 94%, 1.0);
}
```

#### Vendor Prefixes

* Try to use the Bourbon Sass mixins wherever possible. The library should be
  updated periodically.
* Vendor prefixed rules should always precede the global variation of a
  particular rule
* Keep an eye out for obsolete vendor prefixed rules and remove those no longer
  needed.
* When listing vendor prefixed rules, do your best to maintain a layout as
  shown below, and be consistent throughout in your approach

```scss
.selector {
  background-image: -webkit-linear-gradient(#ffffff, #000000);
  background-image:    -moz-linear-gradient(#ffffff, #000000);
  background-image:     -ms-linear-gradient(#ffffff, #000000);
  background-image:      -o-linear-gradient(#ffffff, #000000);
  background-image:         linear-gradient(#ffffff, #000000);
}
```

#### Objects

* Selectors *MUST* always start with the object as the left most selector
