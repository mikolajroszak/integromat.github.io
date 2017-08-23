**Required**: no  
**Default**: `json`  
**Values**: `json`, `urlencoded`, `multipart/form-data`, `text` (or
`string` or `raw`), `binary`.

**Note:** When using `text` (or `string` or `raw`) the body should be a
string. If it is not, it will be converted to string.

**Note 2:** When using `json` or `multipart/form-data` or `urlencoded`
the appropriate value of `Content-Type` header will be set automatically
based on what type you have specified.
