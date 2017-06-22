---
title: API Key, Basic Auth or Digest Auth Connection
layout: apps
---

{% raw %}

Connection is a link between Integromat and 3rd party service/app. Basic connection handles all non-OAuth authorization methods like API Key, Basic Authorization or Digest authorization. The communication section specifies a simple request to determine whether the credentials entered by the user are valid or not. The most common way to validate the credentials is to call an API to get user's information. Most of the APIs have such an API.

## Communication

Specifies connection validation process. This specification does **NOT** inherit from [Base](base.html).

```json
{
    "url": "https://www.example.com/api/whoami",
    "method": "GET",
    "qs": {},
    "body": {},
    "headers": {
        "x-api-key": "{{parameters.apiKey}}"
    },
    "response": {
        "data": {
            "userId": "{{body.userId}}",
            "userName": "{{body.userName}}"
        },
        "metadata": {
            "value": "{{body.userName}}",
            "type": "text"
        },
        "uid": "{{body.userId}}"
    }
}
```

Additional available [Request Object](request-object.html) properties:

Key | Type | Description
--- | --- | ---
**data** | [Complex Object](complex-object.html) | Saves data to connection's data collection for later use. The example above will create two properties `userId` and `userName`, which you can access later in connections via `{{data.userId}}` or in modules and remote procedures via `{{connection.userName}}`.
**metadata** | [Metadata Object](metadata-object.html) | Allows you to save user metadata into connection (like a name or a username). This allows user to recognize correct connection when having multiple of the same type.
**uid** | [IML](iml.html) | An expression that parses user's id from response body. This is necessary when you're using [Shared Webhooks](shared-webhook.html).

## Parameters

Describes structure of input parameters. All parameters are stored to connection's data collection and accessible via `{{connection.variable}}` later. For the communication above the parameters should look like this:

```json
[
    {
        "name": "apiKey",
        "type": "text",
        "label": "API Key",
        "required": true
    }
]
```

See [Parameters Array](parameters-array.html)

## Common Data

A collection to store non-user-specific sensitive values like salts or secrets. Only connections can access this collection.

## Single Request Verification Example

In the below example we connect to a service that requires username and password and returns a token.

```json
{
    "url": "https://www.example.com/api/login",
    "method": "POST",
    "body": {
        "username": "{{parameters.username}}",
        "password": "{{parameters.password}}"
    },
    "response": {
        "data": {
            "token": "{{body.accessToken}}"
        },
        "metadata": {
            "value": "{{body.username}}",
            "type": "text"
        },
        "uid": "{{body.userId}}"
    }
}
```

## Multiple Request Verification Example

In the below example we connect to service that has a two stage login process. First we must obtain a signature from username and apiSecret. Then with this signature and password we obtain a token. We must not forget to delete the user password after we've used it.

```json
[
    {
        "url": "https://www.example.com/api/login/sign",
        "method": "GET",
        "qs": {
            "username": "{{parameters.username}}",
            "apiSecret": "{{parameters.apiSecret}}"
        },
        "response": {
            "temp": {
                "sign": "{{body.signature}}",
                "userId": "{{body.userId}}",
                "username": "{{body.username}}"
            }
        }
    },
    {
        "url": "https://www.example.com/api/login",
        "method": "GET",
        "body": {
            "signature": "{{temp.sign}}",
            "password": "{{parameters.password}}"
        },
        "response": {
            "data": {
                "token": "{{body.accessToken}}"
            },
            "metadata": {
                "value": "{{temp.username}}",
                "type": "text"
            },
            "uid": "{{temp.userId}}"
        }
    }
]
```

{% endraw %}