**Default**: `json`

Available values are `json` (default), `urlencoded`,
`multipart/form-data`, `text` (or `string` or `raw`), `binary`.

**NOTE:** When using `text` (or `string` or `raw`) the body should be a
string. If it is not, it will be converted to string.

**NOTE 2:** When using `json` or `multipart/form-data` or `urlencoded`
the appropriate value of `Content-Type` header will be set automatically
based on what type you have specified.
