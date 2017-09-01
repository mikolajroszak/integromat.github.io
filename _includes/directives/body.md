**Required**: no  
**Default**: empty

This directive specifies the request body when the `method` directive is
set to anything except `GET`. If the body is specified and the `method`
directive is set to `GET` - body is ignored and appropriate
`Content-Type` headers are not set.

Example:
{% raw %}
```json
{
    "url": "http://example.com/post",
    "body": {
        "first_name": "{{parameters.firstName}}",
        "last_name": "{{parameters.lastName}}"
    }
}
```
{% endraw %}

**Note**: If you want to specify XML request body, you can specify it as
a string that will use IML expressions to pass values to XML nodes.
Example:
{% raw %}
```json
{
    "url": "http://example.com/post",
    "body": "<request><rows><row><column name=\"first_name\">{{parameters.firstName}}</column><column name=\"last_name\">{{parameters.lastName}}</column></row></rows></request>"
}
```
{% endraw %}

