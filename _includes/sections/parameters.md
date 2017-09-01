Parameters describe what your module or connection will receive as input
from the user.

```json
[
    {
        "name": "myText",
        "type": "text",
        "label": "Text Field",
        "help": "This is my text field",
        "required": true
    },
    {
        "name": "myNumber",
        "type": "number",
        "label": "Numeric Field",
        "help": "This is my numeric field",
        "advanced": true
    },
    {
        "name": "myBoolean",
        "type": "boolean",
        "label": "Boolean Field",
        "help": "This is my Boolean Field",
        "editable": true
    },
    {
        "name": "myArray",
        "type": "array",
        "label": "Array Field",
        "help": "This is my Array of Strings Field",
        "editable": true
    },
    {
        "name": "myCollection",
        "type": "collection",
        "label": "Collection Field",
        "help": "This is my Collection Field",
        "spec": [
            {
                "name": "greeting",
                "type": "text",
                "label": "Greeting",
                "required": true
            },
            {
                "name": "name",
                "type": "text",
                "label": "Name",
                "required": true
            }
        ]
    }
]
```

For detailed description of parameters and their types see
[Parameters](articles/parameters.html)