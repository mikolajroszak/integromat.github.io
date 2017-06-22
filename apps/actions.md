---
title: Actions
layout: apps
---

{% raw %}

An action is a module that performs a request and returns a single bundle. It should be used for get/insert/update/delete operations. For lists and searches use [Search](search.html) module.

## Communication

Specification of a request/response communication. This specification does inherit from [Base](base.html).

```json
{
    "qs": {},
    "url": "/api/users/create",
    "body": {
        "name": "{{parameters.name}}",
        "email": "{{lower(parameters.email)}}"
    },
    "method": "POST",
    "headers": {},
    "response": {
        "output": {
            "id": "{{body.id}}"
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
        "name": "email",
        "type": "email",
        "label": "Email address",
        "required": true
    },
    {
        "name": "name",
        "type": "text",
        "label": "Name",
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
    }
]
```

Interface uses the same structure as parameters. See [Parameters Array](parameters-array.html)

{% endraw %}