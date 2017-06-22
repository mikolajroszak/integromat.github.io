---
title: OAuth2 Connection
layout: apps
---

{% raw %}

Connection is a link between Integromat and 3rd party service/app. OAuth2 connection handles the token exchange automatically.

## Callback URL

Before you start configuring you OAuth2 connection, you need to create an app on a 3rd-party service. When creating an app, use `https://www.integromat.com/oauth/cb/app` as a callback URL.

## Communication

Specifies token exchange and connection validation process. This specification does **NOT** inherit from [Base](base.html).

```json
{
    "authorize": {
        "qs": {
            "scope": "{{join(oauth.scope, ',')}}",
            "client_id": "{{ifempty(parameters.clientId, common.clientId)}}",
            "redirect_uri": "{{oauth.redirectUri}}",
            "response_type": "code"
        },
        "url": "https://www.example.com/oauth/authorize",
        "response": {
            "temp": {
                "code": "{{query.code}}"
            }
       }
    },
    "token": {
        "url": "https://www.example.com/api/token",
        "body": {
            "code": "{{temp.code}}",
            "client_id": "{{ifempty(parameters.clientId, common.clientId)}}",
            "grant_type": "authorization_code",
            "redirect_uri": "{{oauth.redirectUri}}",
            "client_secret": "{{ifempty(parameters.clientSecret, common.clientSecret)}}"
        },
        "type": "urlencoded",
        "method": "POST",
        "response": {
            "data": {
                "accessToken": "{{body.access_token}}",
                "refreshToken": "{{body.refresh_token}}"
            }
        }
    },
    "info": {
        "url": "https://www.example.com/api/whoami",
        "header": {
            "Authorization": "Bearer {{connection.accessToken}}"
        },
        "response": {
            "uid": "{{body.id}}",
            "metadata": {
                "type": "text",
                "value": "{{body.user}}"
            }
        }
    },
    "refresh": {
        "condition": true,
        "url": "https://www.example.com/oauth/token",
        "body": {
            "client_id":"{{ifempty(parameters.clientId, common.clientId)}}",
            "grant_type":"refresh_token",
            "refresh_token":"{{connection.refreshToken}}"
        },
        "type": "urlencoded",
        "method": "POST",
        "response": {
            "data": {
                "refreshToken": "{{body.refresh_token}}",
                "accessToken": "{{body.access_token}}"
            }
        }
    },
    "invalidate": {
        "url": "https://www.example.com/oauth/invalidate",
        "header": {
            "Authorization": "Bearer {{connection.accessToken}}"
        }
    }
}
```

Key | Type | Description
--- | --- | ---
**authorize** | [Authorize Object](authorize-object.html) | Describes authorization process.
**token** | [Request Object](request-object.html) | Describes a request that exchanges credentials for tokens.
**info** | [Request Object](request-object.html) | Describes a request that validates a connection. The most common way to validate the connection is to call an API's method to get user's information. Most of the APIs have such a method.
**refresh** | [Request Object](request-object.html) | Describes a request that refreshes an access token.
**invalidate** | [Request Object](request-object.html) | Describes a request that invalidates acquired access token.
**preauthorize**| [Request Object](request-object.html) | Describes a request that should be executed prior to `authorize` directive. There are only 2 directives available in `response` section of such request: `temp` and `data`. Everything you save to `temp` you can then use in other OAuth 2 specific sections, while everything you specify in `data` will be saved to connection `data` collection.  

Additional available [Request Object](request-object.html) properties:

Key | Type | Description
--- | --- | ---
**condition** | [IML](iml.html) | Available only in **refresh** directive! Must evaluate to boolean (true or false). An expression that determines if to refresh the token or not. 


Additional available [Response Object](response-object.html) properties:

Key | Type | Description
--- | --- | ---
**data** | [Complex Object](complex-object.html) | Saves data to connection's data collection for later use.
**metadata** | [Metadata Object](metadata-object.html) | Allows you to save user metadata into connection (like a name or a username). This allows user to recognize correct connection when having multiple of the same type.
**uid** | [IML](iml.html) | An expression that parses user's id from response body. This is necessary when you're using [Shared Webhooks](shared-webhook.html).
**expires** | [IML](iml.html) | An expression that parses refresh token's expiration date. Once the token expires, user is requested to reauthorize the connection. **IMPORTANT:** Do not use this to store expiration date of access token.

## Initial flow

Each section is responsible for executing its part in the OAuth 2 flow.

In short, you can describe the initial OAuth 2 flow as follows:
```
preauthorize => authorize => token => info
```
with `preauthorize` and `info` sections being optional, and `refresh` and `invalidate` not being a part of initial OAuth 2 flow.

## Scope List

A key/value collection of all possible scopes user can grant access to. Key represents the scope and values is a description. Example:

```json
{
    "access_user_info": "Permission to read user info.",
    "write_post": "Permission to create posts as a user."
}
```

## Default Scope

Array of minimum scopes user must grant access to. Example:

```json
[
    "access_user_info"
]
```

## Parameters

Describes structure of input parameters. All parameters are stored to connection's data collection and accessible via `{{connection.variable}}` later.  For OAuth2 connections it is possible to allow users to input their own application id and secret. Parameters must be named `clientId` and `clientSecret`. Marking those parameters as advanced is a good practice.

```json
[
    {
        "name": "clientId",
        "type": "text",
        "label": "Client ID",
        "advanced": true
    },
    {
        "name": "clientSecret",
        "type": "text",
        "label": "Client Secret",
        "advanced": true
    }
]
```

See [Parameters Array](parameters-array.html)

## Common Data

A collection to store non-user-specific sensitive values like salts or secrets. Only connections can access this collection.

## Add-ons

If you need to execute a request before authorization (to fetch a token, for example), we got you covered. You can specify a `preauthorize` section, that is a [Request Object](request-object.html), and it will be called just before the `authorize` directive:

```json
{
    "preauthorize": {
        "url": "https://www.example.com/api/key",
        "qs": {
            "client_id": "{{ifempty(parameters.clientId, common.clientId)}}"
        },
        "response": {
            "temp": {
                "key": "{{body.key}}"
            }
        }
    }
}
```

{% endraw %}
