---
title: Remote Procedures
layout: apps
---

{% raw %}

## Specification

```json
{
    "url": "/users",
    "method": "GET",
    "response": {
        "iterate": "{{body.data}}",
        "output": {
            "value": "{{item.id}}",
            "label": "{{item.firstName}} + {{item.lastName}}"
        }
    }
}
```

In order to specify an RPC you use the common [Request Object](request-object.html). The response of the RPC follows common module and RPC [Response Object](response-object.html#Module-and-Remote-Procedure-response-object).

See also [Variables](variables.html), [Integromat Markup Language](iml.html)

## Description

RPC (Remote Procedure Call) allow you to retrieve dynamic data. They have the same specification as [Search](searches.html),
except for the `output` section. There are 4 types of RPCs, and each type has a different output specification.

## Specifying `output` parameter based on type

There are 4 types of remote procedures available in Integromat:
- **Options RPC** - This RPC is used to retrieve items for a dynamic select box
- **Fields RPC** - This RPC is used to retrieve fields for a dynamic form
- **Samples RPC** - This RPC is used to retrieve dynamic sample data
- **Epoch RPC** - This RPC is used to retrieve items for the [Epoch panel](epoch.html). You specify it by overriding specific trigger properties. For more info see [Epoch panel](epoch.html).

### Options RPC

In order to correctly return options from your remote procedure, the `output` section of `response` should contain only 2 items: `label` and `value`

```json
{
    "response": {
        "iterate": "{{body}}",
        "output": {
            "label": "{{item.username}}",
            "value": "{{item.id}}",
			"default": true
        }
    }
}
```

`label` is what the user sees when selecting items from the select box, and `value` is what this field will get after the user selects an item. `default` is optional and when true, option is pre-selected.

**Example usage of select box with dynamic options:**

```json
[
    {
        "name": "param",
        "type": "select",
        "options": "rpc://NameOfMyRemoteProcedure"
    }
]
```

### Fields RPC

Loading fields from a remote server is as easy as adding a couple of properties to `output`, namely `name` and `type`, which are required.
You can also specify any property that is used to specify module [Parameters](parameters-array.html), because conceptually they are the same.
The only difference is that [Parameters](parameters-array.html) are static, and 'Field' RPC is dynamic.

**NOTE**: you should properly convert field types between your service types and Integromat types. For that you could use [Custom IML Functions](functions.html)
 
```json
{
    "response": {
        "iterate": "{{body}}",
        "output": {
            "name": "{{item.key}}",
            "label": "{{item.label}}",
            "type": "text",
            "required": "{{item.isRequired == 1}}"
        }
    }
}
```

**Example usage of dynamic parameters:**

```json
"rpc://NameOfMyRemoteProcedure"
```

**Example usage of select box with nested dynamic parameters:**

```json
[
    {
        "name": "param",
        "type": "select",
        "options": [
            {
                "label": "Option 1",
                "value": 1,
                "nested": "rpc://NameOfMyRemoteProcedure"
            }
        ]
    }
]
```

### Samples RPC

Sample is an object representing one item. If you iterate, don't forget to set limit to 1 so only one item is processed for sample data.

```json
{
    "response": {
        "limit": 1,
        "iterate": "{{body}}",
        "output": "{{item}}"
    }
}
```

**Example usage of dynamic samples:**

```json
"rpc://NameOfMyRemoteProcedure"
```

### Epoch RPC

To correctly return epoch panel items from your remote procedure, the `output` section of `response` should contain only 2 items: `label` and `date`.
The limit is used to limit the number of items displayed to the user when using the Epoch panel.

```json
{
    "response": {
        "limit": 500,
        "iterate": "{{body}}",
        "output": {
            "label": "{{item.name}}",
            "date": "{{item.date_created}}"
        }
    }
}
```

## Parameters

See [Parameters Array](parameters-array.html)

{% endraw %}
