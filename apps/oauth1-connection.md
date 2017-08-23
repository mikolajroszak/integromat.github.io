---
title: OAuth1 Connection
layout: apps
---

{% raw %}

Connection is a link between Integromat and 3rd party service/app. OAuth1 connection handles the token exchange automatically.

## Callback URL

Before you start configuring you OAuth1 connection, you need to create an app on a 3rd-party service. When creating an app, use `https://www.integromat.com/oauth/cb/app` as a callback URL.

## Communication

Specifies token exchange and connection validation process. This specification does **NOT** inherit from [Base](base.html).

```json
{
    "oauth": {
        "consumer_key":"{{ifempty(parameters.consumerKey, common.consumerKey)}}",
        "consumer_secret":"{{ifempty(parameters.consumerSecret, common.consumerSecret)}}"
    },
    "requestToken": {
        "url":"http://www.example.com/oauth/request_token",
        "method":"POST",
        "response":{
            "type":"urlencoded",
            "temp":{
                "token":"{{body.oauth_token}}",
                "token_secret":"{{body.oauth_token_secret}}"
            }
        }
    },
    "authorize": {
        "url":"http://www.example.com/oauth/authenticate",
        "qs":{
            "value":"1.3"
        },
        "oauth":{
            "token":"{{temp.token}}"
        },
        "response":{
            "type":"urlencoded",
            "temp":{
                "token":"{{query.oauth_token}}",
                "verifier":"{{query.verifier}}"
            }
        }
    },
    "accessToken": {
        "url":"http://www.example.com/oauth/access_token",
        "oauth":{
            "token":"{{temp.token}}",
            "token_secret":"{{temp.token_secret}}",
            "verifier":"{{temp.verifier}}"
        },
        "type":"urlencoded",
        "method":"POST",
        "response":{
            "type":"urlencoded",
            "data":{
                "token":"{{body.oauth_token}}",
                "token_secret":"{{body.oauth_token_secret}}"
            }
        }
    },
    "info": {
        "url":"http://www.example.com/api/whoami",
        "oauth":{
            "token":"{{connection.token}}",
            "token_secret":"{{connection.token_secret}}"
        },
        "response":{
            "uid":"{{body.id}}",
            "metadata":{
                "type":"text",
                "value":"{{body.user}}"
            }
        }
    }
}
```

Key | Type | Description
--- | --- | ---
**oauth** | [OAuth1 Object](oauth1-object.html) | Allows you to specify special OAuth1 properties to simplify OAuth1 header generation.
**requestToken** | [Request Object](request-object.html) | Describes a request that retrieves the request token
**authorize** | [Authorize Object](authorize-object.html) | Describes authorization process.
**accessToken** | [Request Object](request-object.html) | Describes a request that exchanges credentials and the request token for the access token.
**info** | [Request Object](request-object.html) | Describes a request that validates a connection. The most common way to validate the connection is to call an API to get user's information. Most of the APIs have such an API.

### The OAuth object

When using an OAuth1 account there is a special object available globally: the `oauth` object. You can use it in account specification as well as in module specification to avoid generating the OAuth1 header yourself. This object is available in the root of account specification, in the [Base](base.html) and in it extends the [Request Object](request-object.html).

If the `oauth` object is present in the root of account specification, it will be merged with each of the directives described above. If you wish to override some properties of the root object, you can do so in the respective directive by specifying the `oauth` object and overriding the properties. 

Key | Type | Description
--- | --- | ---
**oauth** | [OAuth1 Object](oauth1-object.html) | Allows you to specify special OAuth1 properties to simplify OAuth1 header generation.

### Properties

Additional available [Response Object](response-object.html) properties:

Key | Type | Description
--- | --- | ---
**data** | [Complex Object](complex-object.html) | Saves data to connection's data collection for later use.
**metadata** | [Metadata Object](metadata-object.html) | Allows you to save user metadata into connection (like a name or a username). This allows user to recognize correct connection when having multiple of the same type.
**uid** | [IML](iml.html) | An expression that parses user's id from response body. This is necessary when you're using [Shared Webhooks](shared-webhook.html).
**expires** | [IML](iml.html) | An expression that parses refresh token's expiration date. Once the token expires, user is requested to reauthorize the connection. **IMPORTANT:** Do not use this to store expiration date of access token.

## Parameters

Describes structure of input parameters. All parameters are stored to connection's data collection and accessible via `{{connection.variable}}` later.  For OAuth1 connections it is possible to allow users to input their own application id and secret. Parameters must be named `consumerKey` and `consumerSecret`. Marking those parameters as advanced is a good practice.

```json
[
    {
        "name": "consumerKey",
        "type": "text",
        "label": "Consumer Key",
        "advanced": true
    },
    {
        "name": "consumerSecret",
        "type": "text",
        "label": "Consumer Secret",
        "advanced": true
    }
]
```

See [Parameters Array](parameters-array.html)

## Common Data

A collection to store non-user-specific sensitive values like salts or secrets. Only connections can access this collection.

{% endraw %}
