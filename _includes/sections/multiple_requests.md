Making multiple requests is easy - you just have to put all your request
objects in an array. All requests will be executed sequentially and the
output of last request will be used as the module's output.

{% raw %}
**Example**:
```json
[
    {
        "url": "http://example.com/api/user",
        "response": {
            "temp": {
                "username": "{{body.username}}"
            }
        }    
    },
    {
        "url": "http://example.com/items",
        "response": {
            "iterate": "{{body}}",
            "output": {
                "username": "{{temp.username}}",
                "text": "{{item.text}}"
            }
        }
    }
]
```
{% endraw %}