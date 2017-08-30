**Required**: no  
**Default**: empty

This directive let's you customize Integromat's response on the webhook
or a verification request.

| Key                                 | Type                                              | Description                                                       |
| ---                                 | ---                                               | ---                                                               |
| [**`type`**](#iterate-container)    | [IML String](articles/types.md#iml-string)                 | Specifies how data are serialized into body.                      |
| [**`status`**](#iterate-condition)  | [IML String](articles/types.md#iml-string)                 | Specifies the response status code.                               |
| [**`headers`**](#iterate-condition) | [IML Flat Object](articles/types.md#iml-flat-object) | A single level (flat) collection, that specifies request headers. |
| [**`body`**](#iterate-condition)    | Any [IML Type](articles/types.md#iml-types)          | Specifies the response body.                                      |


### `respond.type` {#respond-type}

**Required**: no  
**Default**: `json`  
**Values**: `json`, `urlencoded`, `text`

This directive specifies how to encode data into body.

### `respond.status` {#respond-status}

**Required**: no  
**Default**: empty

This directive specifies the HTTP status code that will be returned with
the response.

### `respond.headers` {#respond-headers}

**Required**: no  
**Default**: empty

This directive specifies custom headers that are to be sent with the
response.

### `respond.body` {#respond-body}

**Required**: no  
**Default**: empty

This directive specifies the response body.