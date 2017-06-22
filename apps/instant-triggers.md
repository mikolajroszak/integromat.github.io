---
title: Instant Triggers
layout: apps
---

{% raw %}

Instant Trigger is a [Trigger](trigger.html) that is executed immediately when the data arrvies to Integromat. There is nothing to configure in this module except interface. The data processing is handled by selected [Webhook](webhook.html).

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