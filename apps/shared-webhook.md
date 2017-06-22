---
title: Shared Webhook
layout: apps
---

{% raw %}

Shared Webhooks power up [Instant Triggers](instant-triggers.html), which execute the flow immediately after the remote server sends data.

Shared Webhooks are different from normal webhooks in that the remote server sends all items to a single URL, instead of multiple URLs for each user.

**IMPORTANT:** In order for shared webhook to work, you always need to create an [Instant Trigger](instant-triggers.html) and pair it with a shared webhook.

## Communication

Specifies how to get data from the payload and how to reply to a remote server.

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
    },
    "respond": {
        "type": "text",
        "status": 200,
        "headers": {
            "Accepted": true
        },
        "body": "accepted"
    },
    "uid": "{{item.userId}}",
    "iterate": "{{body.data}}",
    "output": "{{item}}"
}
```

Key | Type | Description
--- | --- | ---
**respond** | [Respond Object](respond-object.html) | Specifies how to respond to the remote server
**verification** | [Verification Object](verification-object.html) | Specifies how to reply to the remote server, if it needs a confirmation
**uid** | [IML](iml.html) | Since it is a shared webhook, the server should send items, containing user id to which each item belongs to. This directive is used to get the item's user id.
**iterate** | [IML](iml.html) | An expression that points to an array in response body. This allows you to use a special variable `item` that can be used to access properties of iterated items. Available only in [Triggers](triggers.html), [Searches](searches.html) and [Remote Procedures](rpc.html).
**iterate** | [Iterate Object](iterate-object.html) | Allows you to specify iteration condition.
**output** | [Output Object](output-object.html) | Describes structure of the output bundle.


If the webhook returns multiple items in one batch, you might need to use the `iterate` directive to specify which items to output. Then you might want to specify the `output` directive to map items to output. If you do not specify the `output` directive, items will be returned as-is.

## Webhook registration

Registration can be handled either manually by the user or automatically via special attach and detach remote procedures.

### Attach

The attach remote procedure is used to automatically attach a webhook to a remote service.

```json
{
    "url": "https://www.example.com/api/webhook",
    "method": "POST",
    "body": {
        "url": "{{webhook.url}}"
    },
    "response": {
        "data": {
            "externalHookId": "{{body.id}}"
        }
    }
}
```

See [Request Object](request-object.html), [Variables](variables.html)

#### Saving the response data

To save the remote webhook id (in order for detach to work), you must use the `response.data` collection. This collection will be available in the detach webhook remote procedure for you to use.

The `webhook` collection with webhook's data is also accessible in regular remote procedure calls if the call is processed in context of an Instant Trigger. For example if you create a dynamic interface for an Instant Trigger based on parameters entered when webhook was created.

### Detach

The detach remote procedure is used to automatically detach (delete) a webhook from a remote service when it is no longer needed. The only thing you have to do is to correctly specify the url to detach a webhook. No response processing is needed.

```json
{
    "url": "https://www.example.com/api/webhook/{{webhook.externalHookId}}",
    "method": "DELETE"
}
```

See [Request Object](request-object.html), [Variables](variables.html)

{% endraw %}
