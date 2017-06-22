---
title: Temp Object
layout: apps
---

{% raw %}

Collection of variables which can be accessed in other blocks or subsequent requests. Supports nested arrays and objects like [Complex Object](complex-object.html). It is not persistent and the data will be discarded once the execution is done.

## Temp Object Example

This will create a `temp` variable named `signature`...

```json
{
    "response": {
        "temp": {
            "signature": "{{base64(body.token + body.username + body.password)}}"
        }
    }
}
```

... which will be available as `temp.signature` in [IML](iml.html) expressions:

```json
{
    "body": {
        "data": "{{temp.signature}}"
    }
}
```

{% endraw %}