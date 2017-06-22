---
title: Authorize Object
layout: apps
---

{% raw %}

Collection of directives controlling an authorization phase of OAuth2.

Key | Type | Description
--- | --- | ---
**url** | [IML](iml.html) | **Required.** Specifies the URL that the user will be redirected to during the authorization phase.
**qs** | [Flat Object](flat-object.html) | A single level (flat) collection that specifies request query string parameters.
**temp** | [Temp Object](temp-object.html) | Creates/updates variable `temp` which you can access in other blocks. It is not persistent and the data will be discarded once the execution is done.
**response** | [Response Object](response-object.html) | Collection of directives controlling processing of response.
**oauth** | [OAuth1 Object](oauth1-object.html) | Available only in OAuth1 account. Can contain only 1 parameter: `token`, which will be appended to the `url` query string as `oauth_token`. Or you can specify the same `oauth_token` parameter in `qs` manually.

## Authorize Object Example for OAuth 1

```json
{
    "authorize": {
        "url": "http://www.example.com/oauth/authenticate",
        "oauth": {
            "token": "{{temp.token}}"
        },
        "response": {
            "type": "urlencoded",
            "temp": {
                "token": "{{query.oauth_token}}",
                "verifier": "{{query.verifier}}"
            }
        }
    }
}
```

Or

```json
{
    "authorize": {
        "url": "http://www.example.com/oauth/authenticate",
        "qs": {
            "oauth_token": "{{temp.token}}"
        },
        "response": {
            "type": "urlencoded",
            "temp": {
                "token": "{{query.oauth_token}}",
                "verifier": "{{query.verifier}}"
            }
        }
    }
}
```

## Authorize Object Example for OAuth 2

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
    }
}
```

See [Variables](variables.html)

{% endraw %}
