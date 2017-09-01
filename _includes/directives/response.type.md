{% raw %}

**Required**: no  
**Default**: `automatic` (based on `Content-Type` header)  
**Values**: `automatic`, `json`, `urlencoded`, `text` (or `string` or
`raw`), `binary` and `xml`.

This directive specifies how to parse the data received from the server.

When `automatic` is used for the response type, Integromat tries to
parse the response based on it's `Content-Type` header. Currently,
Integromat recognizes only `text/plain`, `application/json`,
`application/x-www-form-urlencoded` and `application/xml`(or
`text/xml`). When specifying other types, Integromat ignores the
`Content-Type` header and tries to parse the response in the format that
you have specified.

Simple example:
```json
{
    "response": {
        "type": "json"
    }
}
```
This will parse all responses as JSON.

You can specify the type as a string, in which case every response, no
matter the status code, will be processed based on your selected type.

##### Custom response types based on status code

You can also specify type as a special object, where keys are status
codes, wildcard or status code ranges. This was made because some
services return different responses with different status codes: JSON on
success and TEXT on error, for example.

**Type object specification**:
- The `*` (wildcard) represents all responses and should be always
present.  
- The `[number]-[number]` represents a status code range
- The `[number]` represents a status code  
  Note that when specifying ranges, when multiple ranges include the
  same status code, the smallest range is selected. The `*` has the
  largest range (1-999) and a number has the smallest range, for example
  `455` is `455-455` range.  
  Ranges are counted inclusive from both sides, so if you specify a
  range `401-402`, both the `401` and `402` status codes will be
  processed by this range.

Example:
```json
{
    "response": {
        "type": {
            "*": "json", // parse all responses as JSON
            "400-408": "text", // parse all 400-408 responses as text, overrides "*",
            "406": "xml" // parse the 406 response as XML, overrides above definitions
        }
    }
}
```
If a response comes with status `406`, it will be parsed as XML  
If a response comes with status `401`, it will be parsed as text  
If a response comes with status `200`, it will be parsed as JSON

##### XML type
Please note that it is not possible to convert XML to JSON objects 1-to-1. That's why there are some tricks to accessing nodes and attributes to parsed XML in Integromat.

###### Everything is an array

First of all, each parsed node is an array, even the root node. Even if in XML a node was single, it will still be represented in Integromat as array. Example:
```xml
<data>
    <text>Hello, world</text>
</data>
```
Will be parsed in Integromat as:
```json
{
    "data": {
        "text": ["Hello, world"]
    }
}
```
So in order to access the value of `<text>` node in, for example, output, one would write this:
```json
{
    "response": {
        "output": "body.data[].text[]"
    }
}
```
Note the `[]` notation. This is a shortcut to get the first element of an array in IML.

###### Accessing value of a node with attributes

If there are attributes present on the node you want to get value of, then you will have to use `_value` on it. Example:
```xml
<data>
    <text foo="bar">Hello, world</text>
</data>
```
Here, the previous example will not work, because the `<text>` node has attributes.
Here is what you might use to access the value of the `<text>` node:
```json
{
    "response": {
        "output": "body.data[].text[]._value"
    }
}
```

###### Accessing node attributes

In a similar manner you can access node attributes with `_attributes`:
```xml
<data>
    <text foo="bar">Hello, world</text>
</data>
```
Note, that `_attributes` is a collection, where you can access each attribute's value by it's name like so:
```json
{
    "response": {
        "output": "body.data[].text[]._attributes.foo"
    }
}
```

###### Accessing nested nodes

When accessing nested XML nodes, one must make sure to access them via the array notation. Also, when accessing nested elements, it doesn't matter if the parent has attributes or no:
```xml
<data>
    <items length="2">
        <item>
            <name>Foo</name>
        </item>
        <item>
            <name>Bar</name>
        </item>
    </items>
</data>
```
If you would then want to process these 2 items in the `iterate` directive, you would then write something like this:
```json
{
    "response": {
        "iterate": "body.data[].items[].item",
        "output": {
            "name": "{{item.name[]}}"
        }
    }
}
```
Please note, that it is the `item` array that contains `<item>` nodes, and not the `items` array.

{% endraw %}