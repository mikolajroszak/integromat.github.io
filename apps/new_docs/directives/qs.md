**Required**: no  
**Default**: empty

This directive specifies the query string to use when making the
request.

**Note**: When specifying this directive, the original query string
specified in the URL will be lost. The `qs` directive takes precedence
over query string specified in the URL. Example:
```json
{
    "url": "http://example.com?foo=bar&baz=qux",
    "qs": {
        "foo": "foobar",
        "hello": "world"
    }
}
```
This will issue a request to this URL:
`http://example.com?foo=foobar&hello=world`. Notice that all the
parameters from the original URL were replaced by the parameters
specified in `qs` directive.

**Note 2**: When specifying `qs` as an empty object, it means that you
want to erase all query string parameters. Example:
```json
{
    "url": "http://example.com?foo=bar&baz=qux",
    "qs": {}
}
```
This will issue a request to this URL: `http://example.com`. Notice that
all the query string parameters from the original URL were removed. That
is because `qs` directive takes precedence over `url` query string
parameters. If you want to specify query string parameters in the `url`,
you should remove the `qs` directive.