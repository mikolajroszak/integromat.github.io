---
title: Verification Object
layout: apps
---

{% raw %}

Verification directive is used in webhooks, where a remote server needs a confirmation that a URL, to wich it will be sending webhooks, is available.

When the request matches the `verification.condition` expression then data will not be processed, but instead Integromat will reply to the remote server, as configured in `verification.respond`

Key | Type | Description
--- | --- | ---
**condition** | [IML](iml.html) | Specifies when a request is a verification request
**respond** | [Respond Object](respond-object.html) | Specifies how to respond to the remote server

## Respond Object Example

```json
{
    "verification": {
		"condition": "{{body.type == 'url_verification'}}",
        "respond": {
            "type": "urlencoded",
            "status": 255,
            "body": {
                "challenge": "{{body.challenge}}"
            }
        }
    }
}
```

{% endraw %}
