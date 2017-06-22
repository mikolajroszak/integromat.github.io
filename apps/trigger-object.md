---
title: Trigger Object
layout: apps
---

{% raw %}

Even if the user can set up a name for the connection, it is always a good practice to add some additional information about the connection. You can specify a [Response Object](response-object.html) section with `metadata` section for that purpose. This allows user to recognize correct connection when having multiple of the same type.

Key | Type | Description
--- | --- | ---
**type** | string | Specifies the type of the trigger, more specifically it points the trigger to the property it should trigger on. Available values: `date` and `id`. If this property is set to `date`, the trigger will mainly use the item date, which the trigger will get via `date` property. If this property is set to `id`, the trigger will use the item id, which the trigger will get via `id` property.
**id** | [IML](iml.html) | **Required**. An expressions that parses unique id from an item.
**date** | [IML](iml.html) | **Required when `type` is set to `date`.** An expressions that parses creation date from an item. You can also specify item update date, but this will impact the trigger behaviour. If the item date property is not in ISO 8601 format, then you need to manually parse it using [IML](iml.html) `parseDate` function like this: `{{parseDate(item.date_created)}}` or `{{parseDate(item.date_created, \"YYYY-DD-MM\")}}`.
**order** | string | Specifies the order in which the API returns items, ordered by a property specified in trigger `type` parameter. Available values are `asc` and `desc` for Ascending and Descending order respectively. If the API returns items, ordered by Date in Ascending order, then trigger `type` should be `date`, and `order` should be `asc`.

**IMPORTANT:** The `id` and `date` directives operate on raw data, and never on the data transformed by the `output` directive!

## Trigger Object Example

```json
{
    "response": {
        "trigger": {
            "id": "{{item.id}}",
            "date": "{{item.created}}",
            "type": "date",
            "order": "desc"
        }
    }
}
```

{% endraw %}
