---
title: Parameters
layout: apps
redirect: https://docs.integromat.com/apps/parameters
---

**For Modules and Connections**:  
Parameters describe what your module will receive as the input from the
user. They describe the fields that the user has to fill on the module
form in order to configure the module.

**For Remote Procedures (RPCs)**:  
Parameters describe what your remote procedure expects to receive in
order to function correctly. The parameters that you specify will be
seen while testing this remote procedure, but when you will be actually
using them, you will have to pass the parameters explicitly.

**Important**: All module parameters are passed to the RPC
automatically. E.g. you have a `firstName` parameter in your module
configuration, it will be available in your RPC as
`parameters.firstName`, just like other parameters passed to the RPC via
the query string.

| Key          | Type    | Description                                                                                                               |
| ---          | ---     | ---                                                                                                                       |
| **name**     | String  | **Required.** Internal parameter name. This is they key in the resulting object.                                          |
| **type**     | Enum    | **Required.** See list of types below.                                                                                    |
| **label**    | String  | Parameter label for the user.                                                                                             |
| **help**     | String  | Parameter description for the user.                                                                                       |
| **default**  | Any     | Specifies the default value of the parameter.                                                                             |
| **advanced** | Boolean | Specifies if the parameter is advanced or not. Advanced parameters are hidden behind a checkbox in GUI. Default: `false`. |
| **required** | Boolean | Specifies if the parameter is required. Default: `false`.                                                                 |

{% raw %}

**Example**:
```json
[
    {
        "name": "param",
        "type": "text",
        "label": "My Parameter",
        "help": "Some help for the parameter.",
        "default": "default-value",
        "advanced": false,
        "required": true
    }
]
```

**PRO TIP 1:** You can mix parameters with RPC URLs to load parts of a
fieldset dynamically.

**PRO TIP 2:** Nested parameters are available for `boolean` and `select` field types.

## Types

- `array` - Array of items of equal type
- `boolean` - `true` or `false`
- `buffer` - Binary data
- `cert` - Certifcate in PEM format
- `collection` - Key-value collection
- `color` - Hex color
- `date` - Date with time
- `email` - Email address
- `filename` - file name
- `filter` - Filtering
- `folder` - Folder selection
- `hidden` - Parameter of this type is hidden from the user. The value
  should always be of type `text`. If the `default` is not `text` then
  it will be converted to `text`.
- `integer` - Whole number
- `number` - Decimal number
///- `password` - Like `text`, just replacing each character with asterisk ("*")
- `path` - A path to a file or a folder
- `pkey`- Private key in PEM format
- `port` - A number in range from 0 to 65535
- `select` - A selection from predefined values
- `text` - Text value
- `time` - Time in `hh:mm` or `hh:mm:ss:` or `hh:mm:ss.nnn` format
- `timestamp` - Unix timestamp
- `timezone` - Time zone name (e.g. `Europe/Prague`)
- `uinteger` - Positive whole number
- `url` - URL address

## Boolean

| Key          | Type    | Description                                                                                                                                                                                           |
| ---          | ---     | ---                                                                                                                                                                                                   |
| **editable** | boolean | If `true`, the user can manually edit the value of this parameter (or use mappings). Default: `false`.                                                                                                |
| **nested**   | string  | Specifies a [Fields RPC](../rpc.md#fields-rpc) URL, that will be called to retrieve nested parameters (fields), that will be shown to the user if this field's checkbox will be checked. |
| **nested**   | array   | Specifies an array of nested parameters (fields), that will be shown to the user if this field's checkbox will be checked.                                                                            |

**Example**:
```json
{
    "name": "myBoolean",
    "type": "boolean",
    "label": "My Boolean",
    "nested": [
        {
            "name": "nestedField1",
            "type": "text",
            "label": "Nested Field 1"
        },
        {
            "name": "nestedField2",
            "type": "text",
            "label": "Nested Field 2"
        }
    ]
}
```

**TIP:** Boolean parameter will be show as three radiobuttons:

![Boolean parameter - radiobuttons](images/boolean-radiobuttons.png)

If you prefer a simple checkbox, add `"required" : true` to the parameter description:
```json
{
    "name": "myBoolean",
    "type": "boolean",
    "label": "My Boolean",
    "required": true,
    ...
}
```

![Boolean parameter - checkbox](images/boolean-checkbox.png)

## Array

| Key                     | Type         | Description                                                                                            |
| ---                     | ---          | ---                                                                                                    |
| **`spec`**              | array/object | Description of items in the array.                                                                     |
| **`editable`**          | boolean      | If `true`, the user can manually edit the value of this parameter (or use mappings). Default: `false`. |
| **`validate`**          | object       | Specifies parameter validation.                                                                        |
| **`validate.maxItems`** | number       | Specifies maximum length that an array parameter can have.                                             |
| **`validate.minItems`** | number       | Specifies minimum length that an array parameter can have.                                             |

**Example of array of numbers:**

```json
{
    "name": "myArray",
    "type": "array",
    "label": "My Array",
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
    "type": "array",
    "label": "My Array",
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
    "type": "collection",
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

To display the Source file set of fields in your module's setup:

![Filename parameter](images/filename.png)

use the following definition:

```json
[
  {
    "name": "fileName",
    "type": "filename",
    "label": "File name",
    "semantic": "file:name"
  },
  {
    "name": "data",
    "type": "buffer",
    "label": "Data",
    "semantic": "file:data"
  }
]
```

## Number, Integer, UInteger, Port

| Key              | Type   | Description                      |
| ---              | ---    | ---                              |
| **validate**     | object | Specifies parameter validation.  |
| **validate.max** | number | Specifies maximum numeric value. |
| **validate.min** | number | Specifies minimum numeric value. |

## Select

| Key          | Type    | Description                                                                                                                        |
| ---          | ---     | ---                                                                                                                                |
| **multiple** | boolean | If `true`, multiple selection is allowed. Default: `false`.                                                                        |
| **editable** | boolean | If `true`, the user can manually edit the value of this parameter (or use mappings). Default: `false`.                             |
| **mode**     | string  | Specifies the initial editing mode when **editable** set to `true`. If `"edit"`, the field will be initially set to **mapping mode**. If `"choose"`, the field will be initially set to **select mode**. Default: `"choose"`. |
| **grouped**  | boolean | If `true`, options can be grouped. Replace **value** with **options** to change the option to a group (see the example below). |
| **options**  | string  | Specifies an [Options RPC](../rpc.md#options-rpc) URL, that will be called to retrieve dynamic options for this RPC.               |
| **options**  | array   | An array of options for this Select. See example for structure definition.                                                         | 
| **options**  | object  | Allows to specify options and nested parameters for this Select field. See below for details.                                      |

if **options** is object, then it specifies options for this select field as well as nested parameters to show when a value has
been selected.

| **options.store**  | string | Specifies an [Options RPC](../rpc.md#options-rpc) URL, that will be called to retrieve dynamic options for this RPC.                                                                      |
| **options.store**  | array  | Specifies options for this Select field.                                                                                                                                                               |
| **options.label**  | string | Specifies the name of a property that will be used as the label of an option.                                                                                                                          | 
| **options.value**  | string | Specifies the name of a property that will be used as the value of an option.                                                                                                                          |
| **options.nested** | array  | Specifies an array of nested parameters (fields), that will be shown to the user if this field's checkbox will be checked.                                                                             |
| **options.nested** | string | Specifies an [Fields RPC](../rpc.md#fields-rpc) URL, that will be called to retrieve nested parameters (fields), that will be shown to the user if this field's checkbox will be checked. |

**Examples**

Select with two options:

```json
{
  "label": "Options AB",
  "name": "optionsAB",
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

Select with two options. The nested fields "Field A1" and "Field A2" will be shown only if the "Option A" has been selected:  

```json
{
  "label": "Options AB",
  "name": "optionsAB",
  "type": "select",
  "options": [
    {
      "label": "Option A",
      "value": "a",
      "nested": [
        {
          "name": "fieldA1",
          "label": "Field A1",
          "type": "text"
        },
        {
          "name": "fieldA2",
          "label": "Field A1",
          "type": "text"
        }
      ]

    },
    {
      "label": "Option B",
      "value": "b"
    }
  ]
}
```

Select with two options. The nested field "Sub Options" will be shown after one of the options has been selected and its content will be fetched dynamically via the RPC call.  

```json
{
  "label": "Options AB",
  "name": "optionsAB",
  "type": "select",
  "options": {
    "store": [
      {
        "label": "Option A",
        "value": "a"
      },
      {
        "label": "Option B",
        "value": "b"
      }
    ],
    "nested": [
      {
        "name": "suboptions",
        "name": "Sub Options",
        "type": "select",
        "options": "rpc://FetchSubOptions"
      }
    ]
  }
}
```

Select with two groups, each with two options:
 
```json
{
  "type": "select",
  "grouped": true,
  "options": [
    {
      "label": "Group A",
      "options": [
        {
          "label": "Option A1",
          "value": "A1"
        },
        {
          "label": "Option A2",
          "value": "A2"
        }
      ]
    },
    {
      "label": "Group B",
      "options": [
        {
          "label": "Option B1",
          "value": "B1"
        },
        {
          "label": "Option B2",
          "value": "B2"
        }
      ]
    }
  ]
}
```

![Select with groups](images/select-group.png)

## Text

| Key                  | Type    | Description                                                                |
| ---                  | ---     | ---                                                                        |
| **multiline**        | boolean | If `true`, user will be able to insert new lines in GUI. Default: `false`. |
| **validate**         | object  | Specifies parameter validation.                                            |
| **validate.max**     | number  | Specifies maximum length.                                                  |
| **validate.min**     | number  | Specifies minumum length.                                                  |
| **validate.enum**    | array   | Specifies an array of items that this parameter can contain.               |
| **validate.pattern** | string  | Specifies a RegExp pattern that a text parameter should conform to.        |

## Filter

| Key                   | Type    | Description                                                                |
| ---                   | ---     | ---                                                                        |
| **options**           | array   | Array of left-side operands. Structure is identical to [Select](#select)'s **options** |
| **options**           | string  | Specifies an [Options RPC](../rpc.md#options-rpc) URL, that will be called to dynamically retrieve list of left-side operands |
| **options**           | object  | Detailed configuration for retrieveing left-side operands |

If **options** is an object, it contains detailed configuration for retrieveing left-side operands.

| **options.store**     | array   | Array of left-side operands. Structure is identical to [Select](#select)'s **options** |
| **options.store**     | string  | Specifies an [Options RPC](../rpc.md#options-rpc) URL, that will be called to dynamically retrieve a list of left-side operands |
| **options.logic**     | string  | If `and`, filters can be combined only with **AND** operator. If `or`, filters can be combined only with **OR** operator. If `both`, both **AND** and **OR** can be used to combine filter. Default: `both`. |
| **options.operators** | array   | Array of operators. Structure is identical to [Select](#select)'s grouped **options** (see option **grouped**). |

Note: If the left-side operands field is not filled, it can be filled manually.

**Examples**

Filter with one left-side operand `email`:

```json
{
  "type": "filter",
  "options": [
    {
      "label": "Email",
      "value": "email"
    }
  ]
}
```

![Filter](images/filter.png)

{% endraw %}