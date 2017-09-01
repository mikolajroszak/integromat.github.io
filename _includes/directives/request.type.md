**Required**: no  
**Default**: `json`  
**Values**: `json`, `urlencoded`, `multipart/form-data`, `text` (or
`string` or `raw`), `binary`.

This directive specifies how the request body will be encoded and send
with the request, when the `method` is anything but `GET`: `POST`, `PUT`,
etc.

When the `method` is `GET` this directive will be ignored.

**Note:** When using `text` (or `string` or `raw`) the body should be a
string. If it is not, it will be converted to string.

**Note 2:** When using `json` or `multipart/form-data` or `urlencoded`
the appropriate value of `Content-Type` header will be set automatically
based on what type you have specified.
