---
title: Metadata Object
layout: apps
---

{% raw %}

Even if the user can set up a name for the connection, it is always a good practice to add some additional information about the connection. You can specify a [Response Object](response-object.html) section with `metadata` section for that purpose. This allows user to recognize correct connection when having multiple of the same type.

Key | Type | Description
--- | --- | ---
**value** | [IML](iml.html) | An expressions that parses the value from response body.
**type** | string | Can be either `text`, `email` or `url`.

## Metadata Object Example

```json
{
    "response": {
        "metadata": {
            "value": "{{body.userName}}",
            "type": "text"
        }
	}
}
```

This is an example of connection without metadata:

![Connection without metadata](images/connection-no-metadata.png)

And this is an example with metadata:

![Connection with metadata](images/connection-metadata.png)

{% endraw %}
