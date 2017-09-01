In every module or connection, you are able to specify 2 directives in
the `response` section that are responsible for handling errors:
`valid` and `error`.

### HTTP 4** and 5** error handling

When the response is returned with 4** or 5** HTTP Status Code, this is
automatically considered as an error. If the `error` directive is not
specified, the user will see a message for the status code that was
returned
([List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_Error)).
But you are able to customize the message, shown to the user with the
`error` or `error.message` directive.

### HTTP 2** and 3** error handling

Some APIs signal about an error with a 200 status code and a flag in the
body. For this scenario, there is a `valid` directive, which tells that
the response is valid or not.

### Custom error handling based on status codes

You are also able to further customize what error message will be shown
to the user based on the status code. To do that, just add your status
code to the `error` directive and fill it in as one:

{% raw %}
```json
{
    "response": {
        "error": {
            "message": "Generic error message",
            "400": {
                "message": "Your request was invalid"
            },
            "500": {
                "message": "The server was not able to handle your request"
            }
        }
    }
}
```

### Examples

Simple error message:

```json
{
    "response": {
        "error": "{{body.message}}"
    }
}
```

Message and type:

```json
{
    "response": {
        "error": {
            "type": "DataError",
            "message": "{{body.message}}"
        }
    }
}
```
{% endraw %}