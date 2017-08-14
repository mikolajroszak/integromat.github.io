---
title: Object Types
layout: apps
---


# Integromat Data Types

Integromat Data Types are derived from normal JSON data types, with some
limitations and additions.

## Primitive Types

### String

A string is a statically specified piece of text, like `"hello, world"`.

### Number

A Number is a sequence of digits, like `8452` or `-123`.

### Boolean

A Boolean is a binary type which has 2 values: `true` or `false`.

### null

`null` is a special type, that represents absence of a value.

## Complex Types

### Flat Object

A Flat Object is a collection of key-value pairs, where key is a
[String](#string), and value can be any
[Primitive Type](#primitive-types). Example:

```json
{
    "id": 1,
    "firstName": "James",
    "lastName": "McManson",
}
```

A Flat Object cannot contain nested collections and arrays.

### Object

An Object is a collection of key-value pairs, where key is a
[String](#string), and value can be any [Primitive](#primitive-types) or
[Complex](#complex-types) type. Example:

```json
{
    "data": [{
        "id": 1,
        "url": "http://example.com"
    }, {
        "id": 2,
        "url": "http://foobar.org"
    }],
    "additional_data": {
        "total": 2,
        "next_page": false,
        "info": null
    }
}
```

### Array

An Array is a collection of [Primitive](#primitive-types) and
[Complex](#complex-types) types.

## IML Types

IML types are special Strings or [Complex Types](#complex-types) that
can contain IML expressions. An IML expression is a template expression
that can resolve into a value.

### IML String

An IML String is a [String](#string) that can contain IML expressions in
between `{{` and `}}` tags. Anything between these tags is considered an
expression. IML Strings are also known as Template Strings. IML String
is an extension to String and, as such, can contain any value that a
normal String can contain. It does not have to be just an IML
expression.

Examples:

A String of a single IML expression: `"{{body.data.firstName + ' ' +
body.data.lastName}}"`

A String without an IML expression: `"Hello, World"`

A String with text and IML expression: `"Hello, {{body.data.name}}"`

### IML Flat Object

An IML Flat Object is a [Flat Object](#flat-object) that can
additionally contain [IML Strings](#iml-string) as values. Example:

```json
{
    "page": "{{temp.page}}",
    "limit": 1,
    "type": "{{parameters.type}}"
}
```

### IML Object

An IML Object is an [Object](#object) that can additionally contain
[IML Strings](#iml-string) and [IML Arrays](#iml-array) as values.
Example:

```json
{
    "data": [{
        "id": "{{body.id}}",
        "name": "{{body.name}}"
    }, "{{parameters.type}}"],
    "info": {
        "pageNumber": 1
    }
}
```

### IML Array

An IML Array is an [Array](#array) that can contain
[IML Strings](#iml-string) and [IML Objects](#iml-object) as values.
Example:

```json
[   
    {
        "id": "{{body.id}}",
        "name": "{{body.name}}",
        "data": {
            "foo": "{{temp.bar}}"
        }
    }, 
    "{{parameters.type}}",
    1,
    true,
    null
]
```

