# ColdFusion

## Style Guide

### Whitesapce

- All source files use one hard tab for each level of indentation
- Don't leave trailing whitespace, including on otherwise blank lines
- Finish all files with a blank line

### Syntax

#### Quotes

- Use double quotes for string values, unless the string value itself contains
  double quotes, when single quotes should be used instead

#### Variable Naming

- Variable names should be in `lowerCamelCase`
- Constant names should be in `UPPERCASE_WITH_UNDERSCORES`
- Don't use variable prefixes (e.g. [Hungarian Notation][hungarian]) - instead
  just name the variables sensibly
- Scopes should be in lowercase: `form.firstName`
- All built-in ColdFusion functions should be in `lowercase()`
- Custom functions and component names should be `lowerCamelCased()`
- Built-in operators should be in uppercase: `EQ`, `NEQ`
- Boolean values should be uppercase: `TRUE` and `FALSE`
- Properties files keys should be in `lowercase_with_underscores`

[hungarian]: http://en.wikipedia.org/wiki/Hungarian_notation

#### Built-in Operators and Syntax

- Use implicit structure and array definition syntax: `{}` and `[]`
- Use captialised syntax for operators: `a EQ b`, not `a == b`
- Do not use the following operators:
  - `EQUAL`, `NOT EQUAL`, `IS`, `IS NOT`
  - `LESS THAN`, `GREATER THAN`
  - `GREATER THAN OR EQUAL TO`, `GE`
  - `LESS THAN OR EQUAL TO`, `LE`

#### Variable Scoping

- All variables must be appropriately scoped
- Use the built-in `local` scope when inside a function

#### Testing for Variable Existence

- Use `structkeyexists()`, not `isdefined()`

#### Boolean Values

- Use `TRUE` or `FALSE` for booleans, not `YES`, `NO`, `1` or `0`
- Use boolean expressions for positive conditions where the explicit notation
  is not necessary

```cfm
<!--- yes --->
<cfif variables.searchResults.recordcount>
<cfif len(variables.firstName)>

<!--- no --->
<cfif variables.searchResults.recordcount GT 0>
<cfif len(variables.firstName) GT 0>
```

- Use explicit boolean expressions for negative conditions for clarity

```cfm
<!--- yes --->
<cfif variables.searchResults.recordcount EQ 0>
<cfif len(variables.firstName) EQ 0>

<!--- no --->
<cfif NOT variables.searchResults.recordcount>
<cfif NOT len(variables.firstName)>
```

#### Security

- Never output any external value without first cleaning it and/or escaping it
  to mitigate against XSS attacks
- Validate and clean any values submitted by the user (`com.wld.generic.XSS`
  can help with this)
- Use the `<cf_csrf_form` custom tag instead of raw `<form` values to ensure
  that form `POST`s are covered by CSRF protection
- Always use `<cfqueryparam>` and the correct `CFSQLType` to mitigate against
  SQL injection attacks
- OWASP provides a [selection of resources][owasp] relevant to best-practices
  surrounding application security for ColdFusion

[owasp]: https://www.owasp.org/index.php/ColdFusion_Security_Resources

#### Using Tags

- Do not use XML-style single-tag closing: use `<cfset ...>` instead of `<cfset
  ... />`
- Avoid switching between tag-based and script-based styles within a single
  block of code

#### Using `<cfscript>`

- Use `<cfscript>` to aid readability, but avoid using it where it introduces
  unnecessary complication (e.g. when performing database queries)
- Confine script-based code to component methods and unit tests
- Curly braces should be preceeded by a space, as should opening parentheses

```cfm
if (expression) { ... }
```

- Function definitions should always include the return type

```cfm
void function doSomething() { ... }
```

- A function's access level should always be stated if not `public`

```cfm
private void function doSomething() { ... }
```

## Principles

- Follow form: if you're modifying an existing section of code, and not
  specifically updating the syntax, keep the formatting like the old code
- All code should look like it's been written by one person
- Redundant variables should be removed from the code
- Do not comment code out: delete it (that's what source control it for)
- Declare variables as close as possible to where they are used
- Use named constants to replace [magic numbers][magic]

[magic]: http://c2.com/cgi/wiki?MagicNumber

## Testing

### Unit Testing

- Unit tests use [MXUnit][mxunit] and [Mockbox][mockbox]
- Test cases should be isolated abd not depend on other objects
- Test components should have a filename ending in `Test.cfc` (e.g.
  `MemberTest.cfc`)
- Reference the component under test as `CUT`

[mxunit]: http://mxunit.org/
[mockbox]: http://wiki.coldbox.org/wiki/MockBox.cfm

```cfm
<cfset variables.CUT = createobject("foo.bar.MyObject")>
```

- Test cases should be named with a `test_` prefix, a descriptive name, with
  words separated by underscores:
  `test_myMethod_returns_true_when_a_is_provided`
- When using `assertEquals()`, the expected value is the _first_ argument
- Do not leave debug calls within a test case
- Write tests in `<cfscript>` where possible

## Preferred Frameworks and Libraries

- Avoid using third-party external libraries where possible
