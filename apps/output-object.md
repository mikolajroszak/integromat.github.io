---
title: Output Object
layout: apps
---

{% raw %}

Describes structure of the output bundle. Supports arrays, nested arrays and objects like [Complex Object](complex-object.html).

## Output Object Example

```json
{
    "response": {
        "output": {
            "id": "{{body.user.id}}",
            "fullName": "{{body.user.firstName}} {{body.user.lastName}}"
        }
    }
}
```

The example above will produce an output which will contain 2 properties: `id` and `fullName`. You can also specify output as `{{body}}` or `{{item}}` to pass all variables to the output automatically. The same behaviour is achieved when `output` is missing.

```json
{
    "response": {
        "output": "{{body}}"
    }
}
```

**NOTE**: Output directive only supports outputting objects and arrays. So if you need to return a single value, you will have to use a wrapper object to wrap that value:

```json
{
    "response": {
        "output": {
            "value": "{{item}}"
        }
    }
}
```

**NOTE 2**: Iterating and outputting arrays is NOT possible. This would mean that you are returning array of arrays:

```json
{
    "response": {
        "iterate": "{{body}}",
        "output": [
            {
                "value": "{{item}}"
            },
            {
                "value": "{{item}}"
            }
        ]
    }
}
```
So you have to either remove the `iterate` directive or remove the array from `output`

{% endraw %}
