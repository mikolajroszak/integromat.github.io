---
title: Triggers
layout: apps
---

{% raw %}

A trigger module is similar to [Search](search.html) module, but unlike search, it stores a state of previsously returned bundles. Thanks to that, only a new records are returned on each execution.

## Communication

Specification of a request/response communication. This specification does inherit from [Base](base.html).

```json
{
    "url": "/api/users",
    "method": "GET",
    "qs": {},
    "body": {},
    "headers": {},
    "response": {
        "trigger": {
            "id": "{{item.id}}",
            "date": "{{item.created}}",
            "type": "date",
            "order": "desc"
        },
        "limit": "{{parameters.limit}}",
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

Additional available [Response Object](request-object.html) properties:

Key | Type | Description
--- | --- | ---
**trigger** | [Trigger Object](trigger-object.html) | Describes how to trigger based on API and items returned from it.

**IMPORTANT:** The trigger directive and all it's sub-directives operate on raw data, and never on the data transformed by the `output` directive!

See [Request Object](request-object.html), [Variables](variables.html), [Pagination](pagination.html)

## Parameters

See [Parameters Array](parameters-array.html)

## Interface

Interface uses the same structure as parameters. See [Parameters Array](parameters-array.html)

{% endraw %}
