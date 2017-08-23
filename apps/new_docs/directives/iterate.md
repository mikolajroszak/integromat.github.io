**Required**: no  
**Default**: empty

This directive is the first step to processing ang outputting multiple
items by your module. It specifies the container for your data and a
filter, ot filter out items that you don't want to be present in the
module output.

In it's simplest form, the `iterate` directive is just an
[IML String](types.md#iml-string). You can also specify this directive
as an object, in which case it will have these directives:

| Key             | Type                              | Description                                                      |
| ---             | ---                               | ---                                                              |
| **`container`** | [IML String](types.md#iml-string) | Specifies the array with the data you want to process            |
| **`condition`** | [IML String](types.md#iml-string) | Specifies a filter that can be used to filter out unwanted items |

**`container`**  
Must point to an array of items that are to be processed.

**`condition`**  
An optional expression to filter out unwanted items.
Must resolve into a [Boolean](types.md#boolean) value, where `true` will
pass the item through, and `false` will drop the item from processing.
The `item` variable is available in this directive.

The `iterate` directive changes the behaviour of the `output` directive
and allows you to use a special variable `item` that represents the
currently processed item. You are able to use this `item` variable in
the `output` directive to access properties of iterated objects. {% if
include.module == "trigger" %} You are also able to access the `item`
variable in `trigger.id` and `trigger.date` directives to specify item
id and item date respectively. {% endif %} For example you are iterating
this response:
```json
{
    "success": true,
    "data": [{
        "id": 1,
        "foo": "bar"
    }, {
        "id": 2,
        "foo": "baz"
    }, {
        "id": 3,
        "foo": "qux"
    }]
}
```
Then, in order to process all items contained in the `data` array, you
would specify your `iterate` directive like so:
{% raw %}
```json
{
    "response": {
        "iterate": "body.data",
        "output": {
            "id": "{{item.id}}",
            "text": "{{item.foo}}"
        }
    }
}
```
{% endraw %}
Here, in the `output` directive, you specify how you wish your output to
look. The `item` variable represents the currently processed item
from the `data` array. The output of this module will then be:
```json
[{
    "id": 1,
    "text": "bar"
}, {
    "id": 2,
    "text": "baz"
}, {
    "id": 3,
    "text": "qux"
}]
```