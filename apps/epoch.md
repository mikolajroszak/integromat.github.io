---
title: Epoch Panel
layout: apps
---

{% raw %}

## Description

The epoch panel is a popup window offered to the user when selecting where to start from when configuring a trigger.

If the `Epoch configuration` is defined, then there will be at least 1 item available in the Epoch panel: `Select`, which will allow 
the user to select an item the user wants to start with. Other items depend on the trigger type.

If the trigger `type` is `id`, then there will be one more option available: `All`, which will allow the user to process
all the items from the beginning. But, if the `Epoch configuration` was not specified, then the Epoch panel will not be available to 
the user.

On the other hand, if the trigger `type` is `date`, then 3 additional options will be available for the user: `All`, `From date`
and `From now`. `From date` will allow the user to start processing items from a specific day forward and from now is the
same as `From date`, except the date is automatically set to current date and time.

**NOTE:**
The data for the `Select` option in the Epoch panel will be retrieved automatically based on the trigger and 
`Epoch configuration`.

## Customizing data retrieval for the Epoch panel

You can customize how the data for the Epoch panel will be retrieved by providing specific request overrides in the `Epoch` section.
These overrides will then be merged with trigger configuration to retrieve the data.

You also have to provide the labels and dates for the returned items. You can do that with with the `response.output` directive ([Output Object](output-object.html)).
Dates are not required, but it would be better to provide them for better user experience. 

Please see [Epoch RPC](remote-procedures.html#epoch-rpc) on how to specify labels and dates for the output.

**Example:**

```json
{
    "qs": {
        "limit": 100
    },
    "response": {
    	"limit": 500,
        "output": {
            "label": "{{item.name}}",
            "date": "{{item.date_created}}"
        }
    },
    "pagination": {
        "qs": "{{temp.page * 100}}"
    }
}
```

In this example the query string `qs.limit` parameter of the original trigger configuration will be overridden 
(if it exists, otherwise it will be added) by the above Epoch configuration and the resulting merged request will be issued to request the data.
Note that `qs.limit` is different from `response.limit`. The `qs.limit` property specifies the maximum number of items 
that will be returned by the remote service in a single request, while the `reponse.limit` specifies how many items will 
be available for the user when using the Epoch panel. Thus, if the remote service has at least 500 items, there will be 
issued in total 5 requests, containing 100 items each. If there will be less then 500 items (number of items will be denotes as `n`), then there will be `⌈n / 100⌉ + 1` requests issued.

{% endraw %}
