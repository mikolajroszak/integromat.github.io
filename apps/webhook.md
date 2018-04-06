---
title: Webhook - Reference Documentation
layout: apps
---

Wehooks power up
[Instant Triggers](trigger.instant.md), which execute the flow
immediately after the remote server sends data.

**IMPORTANT:** In order for webhook to work, you always need to create
an Instant Trigger and pair it with a webhook.

# Index

- [Specification](#specification)
  - [`respond`](#respond)
    - [`status`](#respond-status)
    - [`type`](#respond-type)
    - [`headers`](#respond-headers)
    - [`body`](#respond-body)
  - [`verification`](#verification)
    - [`condition`](#verification-condition)
      - [`condition`](#condition-condition)
      - [`default`](#condition-default)
    - [`respond`](#verification-respond)
  - [`iterate`](#iterate)
    - [`container`](#iterate-container)
    - [`condition`](#iterate-condition)
  - [`output`](#output)
- [Webhook registration](#webhook-registration)
  - [Attach](#attach)
  - [Detach](#detach)

# Specification

Specifies how to get data from the payload and how to reply to a remote server.

```text
{
    "verification": {
        "condition": String|Boolean,
        "respond": {
            "type": Enum[json, urlencoded, text],
            "status": String|Number,
            "headers": Object
            "body": String|Object,
        }
    },
    "respond": {
        "type": Enum[json, urlencoded, text],
        "status": String|Number,
        "headers": Object
        "body": String|Object,
    },
    "iterate": String,
    or
    "iterate": {
        "container": String,
        "condition": String|Boolean
    },
    "output": String|Object
}
```

**Note**:
If the webhook returns multiple items in one batch, you might need to
use the `iterate` directive to specify which items to output. Then you
might want to specify the `output` directive to map items to output. If
you do not specify the `output` directive, items will be returned as-is.


| Key                                 | Type                                                                | Description                                                                     |
| ---                                 | ---                                                                 | ---                                                                             |
| [**`respond`**](#respond)           | Response Specification                                              | Specifies how to respond to the remote server                                   |
| [**`verification`**](#verification) | Verification Specification                                          | Specifies how to reply to the remote server, if it needs a confirmation         |
| [**`iterate`**](#iterate)           | [IML String](articles/types.md#iml-string) or Iterate Specification | Specifies how response items (in case of multiple) are retrieved and processed. |
| [**`output`**](#output)             | Any [IML Type](articles/types.md#iml-types)                         | Describes structure of the output bundle.                                       |

## `respond`

{% include directives/respond.md module="webhook" %}

## `verification`

{% include directives/verification.md module="webhook" %}

## `iterate`

{% include directives/iterate.md module="webhook" %}

## `output`

{% include directives/output.md module="webhook" %}

# Webhook registration

Registration can be handled either manually by the user or automatically
via special
[Attach](rpc.md#attach-rpc) and
[Detach](rpc.md#detach-rpc) remote procedures.

## Attach

The
[Attach remote procedure](rpc.md#attach-rpc) is used to automatically
attach a webhook to a remote service. Please note that you will need to
detach this RPC later, and for that you will need this remote procedure's
Id.

{% raw %}
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
{% endraw %}

To save the remote webhook id (in order for detach to work), you must
use the `response.data` collection. This collection will be available in
the detach webhook remote procedure as `webhook` IML variable for you to use.

The `webhook` collection with webhook's data is also accessible in
regular remote procedure calls if the call is processed in context of an
Instant Trigger. For example if you create a dynamic interface for an
Instant Trigger based on parameters entered when webhook was created.

## Detach

The
[Detach remote procedure](rpc.md#detach-rpc) is used to automatically
detach (delete) a webhook from a remote service when it is no longer
needed. The only thing you have to do is to correctly specify the url to
detach a webhook. No response processing is needed.

{% raw %}
```json
{
    "url": "https://www.example.com/api/webhook/{{webhook.externalHookId}}",
    "method": "DELETE"
}
```
{% endraw %}
