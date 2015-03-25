# Ruby

## Style Guide

The Ruby language has a strong community, and we aim to ensure our code style
lines up with generally-agreed best practices. As such, any Ruby work should
follow the  community-driven style guide available at
[https://github.com/bbatsov/ruby-style-guide][ruby], with the following
clarifications.

[ruby]: https://github.com/bbatsov/ruby-style-guide

### Spacing in Hash Literals

Always add a space after the opening `{` and before the closing `}`:

```ruby
{ one: 1, two: 2 }
```

### Spacing in Embedded Expressions

Don't add spaces around `{` and `}` within embedded expressions:

```ruby
"string#{expr}"
```

### Multi-line Method Chaining

If method chaining spans multiple lines, use a trailing-dot style, with any
additional lines indented appropriately:

```ruby
one.two.three.
  four.five.
  six
```

### Quoting String Literals

All string literals should be quoted with double-quotes unless the string
contains `"` itself, in which case use either `'` or `%Q{}`.

### Hash Literals vs. Keyword Arguments in Method Calls

When making method calls, always use `=>` for hash literals and `:` for keyword
arguments (even when only using one or the other).

```ruby
def foo(bar: "baz", **opts); end

foo(bar: "baz", this: "is", a: "hash")        # bad
foo(bar: "baz", :this => "is", :a => "hash")  # good
```

### Ruby on Rails

For Rails-specific projects, in addition to the guidelines above, follow the
community-driven style guide available at
[https://github.com/bbatsov/rails-style-guide][rails].

[rails]: https://github.com/bbatsov/rails-style-guide

## Testing

Unit and integration tests should be performed using `RSpec`. When writing test
cases, the guidelines at [http://betterspecs.org/](http://betterspecs.org/)
should be followed.
