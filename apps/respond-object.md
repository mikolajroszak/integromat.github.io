---
title: Respond Object
layout: apps
---

{% raw %}

Respond directive is used in webhooks to specify how Integromat replies to the remove server after receiving a webhook request.

Key | Type | Description
--- | --- | ---
**type** | string | Specifies how data are serialized into body. Available values: `json` (default), `urlencoded`, `binary`.
**status** | [IML](iml.html) | HTTP status code with which the reply will be sent.
**headers** | [Flat Object](flat-object.html) | A single level (flat) collection, that specifies request headers.
**body** | [Complex Object](complex-object.html) | Specifies reply body. The value of this property can be an arbitrary JSON object or a primitive: a number, a boolean, a string, an object, an array. This property will undergo IML transformation, replacing IML expressions with values and the result will be send as the request payload.

## Respond Object Example

```json
{
    "respond": {
		"type": "text",
        "status": 200,
        "headers": {
            "Accepted": true
        },
        "body": "accepted"
    }
}
```

## Available IML properties

There are 2 additional properties available to every IML expression within the `response` directive:
- `body` - Represents request body
- `headers` - Represents request headers

{% endraw %}
