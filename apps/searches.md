---
title: Searches
layout: apps
---

{% raw %}

A search module is an [Action](action.html) capable of returning multiple bundles. For single-bundle operations like get/insert/update/delete use [Action](action.html) module.

## Communication

Specification of a request/response communication. This specification does inherit from [Base](base.html).

```json
{
    "url": "/api/users",
    "method": "GET",
    "qs": {
        "search": "{{parameters.name}}"
    },
    "body": {},
    "headers": {},
    "response": {
        "iterate": "{{body.users}}",
        "output": {
            "id": "{{item.id}}",
            "name": "{{item.name}}",
            "email": "{{item.email}}",
            "created": "{{item.created}}"
        }
    }
}
```

See [Request Object](request-object.html), [Variables](variables.html), [Pagination](pagination.html)

## Parameters

Describes structure of input parameters. For the communication above the parameters should look like this:

```json
[
    {
        "name": "search",
        "type": "text",
        "label": "Search",
        "required": true
    }
]
```

See [Parameters Array](parameters-array.html)

## Interface

Describes structure of output bundles. For the communication above the interface should look like this:

```json
[
    {
        "name": "id",
        "type": "uinteger",
        "label": "User ID"
    },
    {
        "name": "email",
        "type": "email",
        "label": "Email address"
    },
    {
        "name": "name",
        "type": "text",
        "label": "Name"
    },
    {
        "name": "created",
        "type": "date",
        "label": "Date created"
    }
]
```

Interface uses the same structure as parameters. See [Parameters Array](parameters-array.html)

{% endraw %}