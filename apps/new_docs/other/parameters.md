---
title: Parameters Array
layout: apps
---

The Parameters Array describes what your module will receive as the
input from the user. It describes the fields that the user has to fill
on the module form in order to configure the module.

{% raw %}

```json
[
    {
        "name": "param",
        "label": "My Parameter",
        "help": "Some help for the parameter.",
        "type": "text",
        "required": true,
        "default": "default-value"
    }
]
```

**PRO TIP:** You can mix parameters with RPC URLs to load parts of a
fieldset dynamically.

| Key          | Type    | Description                                                                                                               |
| ---          | ---     | ---                                                                                                                       |
| **name**     | string  | **Required.** Internal parameter name. This is they key in the resulting object.                                          |
| **type**     | enum    | **Required.** See list of types below.                                                                                    |
| **label**    | string  | Parameter label for the user.                                                                                             |
| **help**     | string  | Parameter description for the user.                                                                                       |
| **default**  | any     | Specifies the default value of the parameter.                                                                             |
| **advanced** | boolean | Specifies if the parameter is advanced or not. Advanced parameters are hidden behind a checkbox in GUI. Default: `false`. |
| **required** | boolean | Specifies if the parameter is required. Default: `false`.                                                                 |

## Types

- `array` - Array of items of equal type
- `boolean` - `true` or `false`
- `buffer` - Binary data
- `collection` - Key-value collection
- `color` - Hex color
- `date` - Date with time
- `email` - Email address
- `file` - File selection
- `filename` - file name
- `folder` - Folder selection
- `hidden` - Parameter of this type is hidden from the user. The value
  should always be of type `text`. If the `default` is not `text` then
  it will be converted to `text`.
- `integer` - Whole number
- `number` - Decimal number
- `path` - A path to a file or a folder
- `port` - A number in range from 0 to 65535
- `select` - A selection from predefined values
- `text` - Text value
- `time` - Time in `hh:mm` or `hh:mm:ss:` or `hh:mm:ss.nnn` format
- `timestamp` - Unix timestamp
- `timezone` - Time zone name (e.g. `Europe/Prague`)
- `uinteger` - Positive whole number
- `url` - URL address

## Array

| Key                   | Type         | Description                                                |
| ---                   | ---          | ---                                                        |
| **spec**              | array/object | Description of items in the array.                         |
| **validate**          | object       | Specifies parameter validation.                            |
| **validate.maxItems** | number       | Specifies maximum length that an array parameter can have. |
| **validate.minItems** | number       | Specifies minimum length that an array parameter can have. |

**Example of array of numbers:**

```json
{
    "name": "myArray",
    "label": "My Array",
    "type": "array",
    "spec": {
        "label": "Numeric value",
        "type": "number"
    }
}
```

**Example of array of complex objects:**

```json
{
    "name": "myArray",
    "label": "My Array",
    "type": "array",
    "spec": [
        {
            "name": "email",
            "type": "email"
        },
        {
            "name": "phone",
            "type": "text"
        }
    ]
}
```

## Collection

| Key      | Type  | Description                                                      |
| ---      | ---   | ---                                                              |
| **spec** | array | Description of the collection. Should be an array of parameters. |

**Example**

```json
{
    "name": "myCollection",
    "label": "My Collection",
    "spec": [
        {
            "name": "email",
            "type": "email"
        },
        {
            "name": "phone",
            "type": "text"
        }
    ]
}
```

## Date

| Key      | Type    | Description                                                            |
| ---      | ---     | ---                                                                    |
| **time** | boolean | If `false`, the GUI will only display date selection. Default: `true`. |

## Filename

| Key           | Type         | Description              |
| ---           | ---          | ---                      |
| **extension** | string/array | Allowed file extensions. |

**Example**

```json
{
    "name": "myFileName",
    "label": "My File Name",
    "extension": ["png", "jpg", "jpeg"]
}
```

## Number

| Key              | Type   | Description                      |
| ---              | ---    | ---                              |
| **validate**     | object | Specifies parameter validation.  |
| **validate.max** | number | Specifies maximum numeric value. |
| **validate.min** | number | Specifies minimum numeric value. |

## Select

| Key          | Type         | Description                                                                                                                              |
| ---          | ---          | ---                                                                                                                                      |
| **multiple** | boolean      | If `true`, multiple selection is allowed. Default: `false`.                                                                              |
| **editable** | boolean      | If `true`, the user can manually edit the value fo this parameter (or use mappings). Default: `false`.                                   |
| **options**  | array/string | An array of items. See example for structure definition. May be a [Options RPC](remote-procedures.html) URL for dynamic content loading. |

**Example**

```json
{
    "name": "field",
    "type": "select",
    "options": [
        {
            "label": "Option A",
            "value": "a"
        },
        {
            "label": "Option B",
            "value": "b"
        }
    ]
}
```

## Text

| Key                  | Type    | Description                                                                |
| ---                  | ---     | ---                                                                        |
| **multiline**        | boolean | If `true`, user will be able to insert new lines in GUI. Default: `false`. |
| **validate**         | object  | Specifies parameter validation.                                            |
| **validate.max**     | number  | Specifies maximum length.                                                  |
| **validate.min**     | number  | Specifies minumum length.                                                  |
| **validate.enum**    | array   | Specifies an array of items that this parameter can contain.               |
| **validate.pattern** | string  | Specifies a RegExp pattern that a text parameter should conform to.        |

{% endraw %}