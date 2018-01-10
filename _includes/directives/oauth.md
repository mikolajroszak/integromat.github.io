It is sometimes very tedious and hard to generate an OAuth1
Authorization header. So we have provided a helper directive, that will
simplify this task. Below are all the properties that you can use to
customize the header generation.

{% if include.module != "connection" %}
**Note**: This directive is effective only when the module's connection
is OAuth 1. Otherwise it will generate Invalid Headers which might lead
to invalid requests being made.
{% endif %}

| Key                  | Type                                        | Description                                                                                                                                              |
| ---                  | ---                                         | ---                                                                                                                                                      |
| **consumer_key**     | [IML String](articles/types.md#iml-string)  | You consumer key                                                                                                                                         |
| **consumer_secret**  | [IML String](articles/types.md#iml-string)  | Your consumer secret                                                                                                                                     |
| **private_key**      | [IML String](articles/types.md#iml-string)  | Instead of `consumer_secret` you can specify a `private_key` string in [PEM format](http://how2ssl.com/articles/working_with_pem_files/)                 |
| **token**            | [IML String](articles/types.md#iml-string)  | An expression that parses the `oauth_token` string.                                                                                                      |
| **token_secret**     | [IML String](articles/types.md#iml-string)  | An expression that parses the `oauth_token_secret` string.                                                                                               |
| **verifier**         | [IML String](articles/types.md#iml-string)  | An expression that parses the `oauth_verifier` string.                                                                                                   |
| **signature_method** | [String](articles/types.md#string)          | Specifies the desired method to use when calculating the signature. Can be either `HMAC-SHA1`, `RSA-SHA1`, `PLAINTEXT`. Default is `HMAC-SHA1`.          |
| **transport_method** | [String](articles/types.md#string)          | Specifies how OAuth parameters are sent: via query params, header or in a POST body. Can be either `query`, `body` or `header`. Default is `header`      |
| **body_hash**        | [IML String](articles/types.md#iml-string)  | To use Request Body Hash, you can either manually generate it, or you can set this directive to `true` and the body hash will be generated automatically |