---
title: Instant Triggers
layout: apps
---

{% raw %}

Instant Trigger is a [Trigger](trigger.html) that is executed immediately when the data arrives to Integromat. There is nothing to configure in this module except interface. The data processing is handled by selected [Webhook](webhook.html).

## Fetch additional data for each bundle

Describes a request to execute for each bundle of the web hook:

```json
{
    "url": "http://example.com/api/item/{{payload.id}}",
    "response": {
        "output": {
          "id": "{{payload.id}}",
          "data": "{{body}}"
        }
    }
}
```

This is a normal [Request Object](request-object.html), except that `iterate` and `pagination` directives are disabled and
there is one more [IML](iml.html) variable added - `payload`, which represents current web hook item that is being processed.

**NOTE:** You are able to specify only a single request currently.

## Interface

Describes structure of output bundles. Example:

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
    }
]
```

Interface uses the same structure as parameters. See [Parameters Array](parameters-array.html)

{% endraw %}