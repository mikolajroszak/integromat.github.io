**Required**: no  
**Default**: empty

This directive specifies headers that will be sent with the request.
All header names are case insensitive, so `x-requested-with` is the same
as `X-Requested-With`.

Example:
```json
{
    "url": "http://example.com/data",
    "headers": {
        "X-Item-Id": "{{parameters.id}}"
    }
}
```
