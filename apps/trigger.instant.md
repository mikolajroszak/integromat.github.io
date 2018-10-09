---
title: Instant Trigger Module - Reference Documentation
layout: apps
redirect: https://docs.integromat.com/apps/app-structure/modules/instant-trigger
---

{% raw %}

Instant Trigger is a Trigger that is executed immediately when the data
arrives to Integromat. There is nothing to configure in this module
except interface. The data processing is handled by a selected
[Webhook](webhook.md).

### Retrieving additional data for each bundle

If you need to retrieve additional data for each bundle, you can
describe a request to execute for each bundle of the webhook:

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

This is a normal
[Request Specification](trigger.md#making-requests) object, except that
`iterate` and `pagination` directives are disabled and there is one more
[IML variable](trigger.md#iml-variables) added - `payload`, which
represents current web hook item that is being processed.

**NOTE:** You are currently able to specify only a single request.

{% endraw %}