When you need the module to output some static (or computed) content, you
may use the Request-less/Static mode by simply omitting the `url` and
specifying only the `response.output` directives.

You may mix static definitions with normal requests and can have more
than 2 static requests.

When using this mode, the following directives are completely ignored:
`method`, `qs`, `headers`, `body`, `ca`, `type` and `pagination` -
almost all request-related directives.

All response-related directive, such as `response.output`,
`response.wrapper`, `response.iterate` are available, as well as
`response.valid` and `response.error`. The latter directives lose their value
in static mode, though.

{% raw %}
**Example**:
```json
{
    "response": {
        "output": {
            "id": "{{parameters.itemId}}",
            "text": "[{{parameters.itemId}}] {{parameters.text}}"
        }
    }
}
```

**Example** with different outputs depending on condition:
```json
[{
    "condition": "{{parameters.mode == 'self'}}",
    "response": {
        "output": {
            "text": "No items"
        }
    }
}, {
    "condition": "{{parameters.mode != 'self'}}",
    "response": {
        "output": {
            "text": "Some items found"
        }
    }
}]
```
{% endraw %}