**Required**: no  
**Default**: empty

This directive allows you to reply to webhook verification requests.
Some systems will not allow you to create webhooks prior to verifying
that the remote side (in this case Integromat) is prepared to handle
them. Such systems may send a code and request Integromat to return it
and may be some other value with it. In such case this directive will
help you.

| Key                                 | Type                                              | Description                                                       |
| ---                                 | ---                                               | ---                                                               |
| [**`condition`**](#verification-condition)    | [IML String](articles/types.md#iml-string)                 | Specifies how data are serialized into body.                      |
| [**`respond`**](#verification-respond)  | [IML String](articles/types.md#iml-string)                 | Specifies the response status code.                               |
| [**`headers`**](#iterate-condition) | [IML Flat Object](articles/types.md#iml-flat-object) | A single level (flat) collection, that specifies request headers. |
| [**`body`**](#iterate-condition)    | Any [IML Type](articles/types.md#iml-types)          | Specifies the response body.                                      |

**Example**:
```json
{
    "verification": {
        "condition": "{{if(body.code, true, false)}}",
        "respond": {
            "status": 202,
            "type": "json",
            "body": {
                "code": "{{body.code}} | 123abc"
            }
        }
    }
}
```

### `verification.condition` {#verification-condition}

**Required**: no  
**Default**: empty

This directive distinguishes normal webhook requests from verification
requests. Usually, the remote service will send some kind of code to verify
that Integromat is capable of receiving data. In such case you may want
to check for the existence of this code variable in the request body.
If it exists - this means that this request is verification request.
Otherwise it may be a normal webhook request with data.

### `verification.respond` {#verification-respond}

**Required**: no  
**Default**: empty

This directive is exactly the same as the [`respond`](#respond)
directive, except that it is nested in `verification`. The behaviour of
`verification.resond`, is the same as normal `respond`,
