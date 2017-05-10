# ColdFusion

## Style Guide

### Whitespace

- All source files use one hard tab for each level of indentation
- Don't leave trailing whitespace, including on otherwise blank lines
- Use a single blank line for code spacing instead of multiple blank lines
- Finish all files with a blank line
- Add a single space after commas (`, `) and around equals symbols (` = `)

### Syntax

#### Method Invocation

- Use named arguments

#### Quotes

- Use double quotes for string values, unless the string value itself contains
  double quotes, when single quotes should be used instead

#### Variable Naming

- Variable names should be in `lowerCamelCase`
- Constant names should be in `UPPERCASE_WITH_UNDERSCORES`
- Don't use variable prefixes (e.g. [Hungarian Notation][hungarian]) - instead
  just name the variables sensibly
- Variable names should be descriptive without being too verbose:
  `memberId` not `id`
- Variable names should reflect the purpose of a variable in isolation
- Properties files keys should be in `lowercase_with_underscores`

[hungarian]: http://en.wikipedia.org/wiki/Hungarian_notation

#### Creating Objects

- The correct case should be used when creating objects. For example, if a
  component has the path `the/path/to/MyObject.cfc` the object should be created
  as `createobject("the.path.to.MyObject")` and not
  `createobject("the.path.to.myobject")`.

#### Function Naming

- All built-in ColdFusion functions should be in `lowercase()`
- Custom functions and component names should be `lowerCamelCased()`

#### Built-in Operators and Syntax

- Use implicit structure and array definition syntax: `{}` and `[]`
- Built-in operators should be in uppercase: `EQ`, `NEQ`
- Use capitalised syntax for operators: `a EQ b`, not `a == b`
- Do not use the following operators:
  - `EQUAL`, `NOT EQUAL`, `IS`, `IS NOT`
  - `LESS THAN`, `GREATER THAN`
  - `GREATER THAN OR EQUAL TO`, `GE`
  - `LESS THAN OR EQUAL TO`, `LE`

#### Variable Scoping

- All variables must be appropriately scoped
- Scopes should be in lowercase: `form.firstName`
- Use the built-in `local` scope when inside a function

#### Testing for Variable Existence

- Use `structkeyexists()`, not `isdefined()`

#### Boolean Values

- Boolean values should be uppercase: `TRUE` and `FALSE`, except for settings
  with boolean attributes which should be lowercase `true` and `false`.
  e.g.`output` and `required` attributes.
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
- Validate and clean any values submitted by the user
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
- Curly braces should be preceded by a space, as should opening parentheses

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

## `CFSQLType` Mappings For MySQL 5

Use the appropriate `CFSQLType` value when using `<cfqueryparam>`.

| ColdFusion 9 | MySQL 5.1.42Connector/J 5.1.7 |
| ------------ |:------------:|
| CF_SQL_ARRAY | - |
| CF_SQL_BIGINT | bigint (signed), int (unsigned) |
| CF_SQL_BINARY | binary, tinyblob |
| CF_SQL_BIT | bit, bool |
| CF_SQL_BLOB | blob |
| CF_SQL_CHAR | char |
| CF_SQL_CLOB | - |
| CF_SQL_DATE | date |
| CF_SQL_DECIMAL | decimal |
| CF_SQL_DISTINCT | - |
| CF_SQL_DOUBLE | double, double precision, real |
| CF_SQL_FLOAT | - |
| CF_SQL_IDSTAMP | - |
| CF_SQL_INTEGER | mediumint (signed and unsigned), int (signed) |
| CF_SQL_LONGVARBINARY | mediumblob, longblob, tinyblob |
| CF_SQL_LONGVARCHAR | text, mediumtext,longtext |
| CF_SQL_MONEY | - |
| CF_SQL_MONEY4 | - |
| CF_SQL_NULL | - |
| CF_SQL_NUMERIC | numeric, bigint(unsigned) |
| CF_SQL_OTHER | - |
| CF_SQL_REAL |  |
| CF_SQL_REFCURSOR | - |
| CF_SQL_SMALLINT | smallint (signed or unsigned), tinyint (signed) |
| CF_SQL_STRUCT | - |
| CF_SQL_TIME | * |
| CF_SQL_TIMESTAMP | datetime, timestamp |
| CF_SQL_TINYINT | tinyint (unsigned) |
| CF_SQL_VARBINARY | varbinary |
| CF_SQL_VARCHAR | varchar, tinytext, enum, set |

Source: http://cfsearching.blogspot.co.uk/2010/01/cfqueryparam-matrix-for-mysql-5.html

## Principles

- Follow form: if you're modifying an existing section of code, and not
  specifically updating the syntax, keep the formatting like the old code
- All code should look like it's been written by one person
- Redundant variables should be removed from the code
- Do not comment code out: delete it (that's what source control is for)
- Declare variables as close as possible to where they are used
- Use named constants to replace [magic numbers][magic]

[magic]: http://c2.com/cgi/wiki?MagicNumber

## Testing

### Unit Testing

- Unit tests use [MXUnit][mxunit] and [Mockbox][mockbox]
- Test cases should be isolated and not depend on other objects
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
