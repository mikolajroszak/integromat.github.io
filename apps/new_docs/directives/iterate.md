**Required**: no  
**Default**: empty

This directive specifies the container of an array of items that the
module must process and output. In its simplest form `iterate` directive
is an [IML String](types.md#iml-string), which points to a container of
your items (must be an array):

{% raw %}
```json
{
    "response": {
        "iterate": "{{body.data}}"
    }
}
```
{% endraw %}

When you need to filter out some items from processing, you are able to
specify the `iterate` directive as an object, in which case it will have
the following properties:

| Key                                   | Type                              | Description                                                      |
| ---                                   | ---                               | ---                                                              |
| [**`container`**](#iterate-container) | [IML String](types.md#iml-string) | Specifies the array with the data you want to process            |
| [**`condition`**](#iterate-condition) | [IML String](types.md#iml-string) | Specifies a filter that can be used to filter out unwanted items |

### `iterate.container` {#iterate-container}

**Required**: yes  
**Default**: empty

The `iterate.container` directive must point to an array of items that
are to be processed.

### `condition` {#iterate-condition}

**Required**: no  
**Default**: empty

An optional expression to filter out unwanted items.
Must resolve into a [Boolean](types.md#boolean) value, where `true` will
pass the item through, and `false` will drop the item from processing.
The `item` variable is available in this directive, which represents
the current item being processed.

**Important**:  
The `iterate` directive changes the behaviour of the `output` directive
and allows you to use a special variable `item` that represents the
currently processed item. The `output` directive will be executed for
each item in the container that you have specified in
`iterate.container`. You are able to use the `item` variable in the
`output` directive to access properties of iterated objects.
{% if include.module == "trigger" %}
You are also able to access the `item` variable in `trigger.id` and
`trigger.date` directives to specify item id and item date respectively.
{% endif %}

For example you are iterating this response:
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