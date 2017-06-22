---
title: OAuth1 Object
layout: apps
---

{% raw %}

It is very tedious and hard to generate an OAuth1 Authorization header. So we have provided a helper directive, that will simplify this task. Below are all the properties that you can use to customize the header generation. 

Key | Type | Description
--- | --- | ---
**consumer_key** | [IML](iml.html) | You consumer key
**consumer_secret** | [IML](iml.html) | Your consumer secret
**private_key** | [IML](iml.html) | Instead of `consumer_secret` you can specify a `private_key` string in [PEM format](http://how2ssl.com/articles/working_with_pem_files/)
**token** | [IML](iml.html) | An expression that parses the `oauth_token` string.
**token_secret** | [IML](iml.html) | An expression that parses the `oauth_token_secret` string.
**verifier** | [IML](iml.html) | An expression that parses the `oauth_verifier` string.
**signature_method** | string | Specifies the desired method to use when calculating the signature. Can be either `HMAC-SHA1`, `RSA-SHA1`, `PLAINTEXT`. Default is `HMAC-SHA1`.
**transport_method** | string | Specifies how OAuth parameters are sent: via query params, header or in a post body. Can be either `query`, `body` or `header`. Default is `header`
**body_hash** | [IML](iml.html) | To use Request Body Hash, you can either manually generate it, or you can set this directive to `true` and the body hash will be generated automatically 

## OAuth Object Example

```json
{
    "oauth": {
        "consumer_key": "1293561234",
        "consumer_secret": "1m34287rn19384ctn203",
        "token": "{{query.oauth_token}}",
        "token_secret": "{{query.oauth_token_secret}}",
        "verifier": "{{query.oauth_verifier}}"
	}
}
```

{% endraw %}
