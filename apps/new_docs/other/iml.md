---
title: Integromat Markup Language
layout: apps
---

{% raw %}

IML is a mustache-like markup language with support for evaluating
complex expressions in javascript-like syntax.

Expressions are written between mustaches: `{{body.data}}`.

## Available operators

### Algebraic operators

`+`, `-`, `*`, `/`, `%`

### Logical operators

`==` (or `===`, same thing), `<`, `<=`, `>`, `>=`, `!=`

### Strings inside expressions

you can use `'` or `"` to put strings inside IML expressions:

```text
{{'Hello, ' + item.name}}
{{"Hello, " + item.name}}
```

### Conditionals

You can create conditionals with `if` and `ifempty` functions.

```text
{{if(body.id > 100, body.user, false)}}
{{ifempty(body.username, body.user)}}
```

`ifempty` function returns second argument when first argument is either
not defined, null or empty string. Otherwise it returns first argument.

### Retrieving collection properties

You can use normal JavaScript `.` (dot) notation to retrieve properties
of collections, such as `parameters`: `{{parameters.input.data}}`.
 
If the property name contains spaces or starts with a non-alphabetic
character or contains non-alphanumeric characters (except underscore
`_`), you can use `` ` `` (back ticks) to retrieve such properties:
``{{headers.`X-HOOK-TYPE`}}`` or ``{{parameters.`First Name`}}``.

### Retrieving array items

You can use normal JavaScript `[]` brackets notation to retrieve a
specific element from an array: `{{body.data[2].prop}}`. Unlike
JavaScript, IML uses `1` to access the first item in an array.

You can also use `-1` to access the last element of an array, like so:
`body.data[-1].prop`.

**Tip**: If you need to retrieve a property of an object, and the
property name is contained in a variable, or computed, the dot `.`
notation will not work. For this you can use the `get` function, where
the first argument of the function is the object you wish to retrieve
the property from, and the second argument is the property name, or a
variable containing a property name, or an expression that resolves to
a property name: `get(body.data, 'some property')`,
`get(body, parameters.fieldName)`,
`get(body.data, if(body.xml == true, 'xml', 'json'))`

### Built-in functions

List of built-in IML functions is available
[here](https://www.integromat.com/en/kb/functions.html#string-functions).

See also [Custom IML Functions](functions.md)

{% endraw %}