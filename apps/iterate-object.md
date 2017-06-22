---
title: Iterate Object
layout: apps
---

{% raw %}

Controls the behavior of [Response Object](response-object.html)'s iteration. This directive is available only in [Triggers](triggers.html), [Searches](searches.html) and [Remote Procedures](rpc.html).

Key | Type | Description
--- | --- | ---
**condition** | [IML](iml.html) | An expression that specifies the condition, which should be met by an item to be included in the output.
**container** | [IML](iml.html) | An expression that points to an array in response body. This allows you to use a special variable `item` that can be used to access properties of iterated items.

## Iterate Object Example

```json
{
    "response": {
        "iterate": "{{body.data.users}}",
        "output": {
            "id": "{{item.id}}",
            "fullName": "{{item.firstName + item.lastName}}"
        }
    }
}
```

Iteration with condition:

```json
{
    "response": {
        "iterate": {
            "container": "{{body.data.users}}",
            "condition": "{{item.moderator == false}}"
        },
        "output": {
            "id": "{{item.id}}",
            "fullName": "{{item.firstName + item.lastName}}"
        }
    }
}
```

{% endraw %}