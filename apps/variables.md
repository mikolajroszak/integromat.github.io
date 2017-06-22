---
title: Variables
layout: apps
---

{% raw %}

Variables available everywhere

- **`now`** - Current date and time
- **`environment`** - TBD
- **`temp`** - Contains custom variables created via `temp` directive in [Request Object](request-object.html) or [Response Object](response-object.html).

Variables available in connections

- **`parameters`** - Contains connection's input parameters.
- **`common`** - Contains connection's common data collection.
- **`data`** - Contains connections's data collection.
- **`oauth.scope`** - Contains an array of scope required to be passed to OAuth2 authorization process.
- **`oauth.redirectUri`** - Contains redirect URL for OAuth2 authorization process.
- **`query`** - Contains query parameters returned by a 3rd party during OAuth2 authorization.

Variables available in modules

- **`parameters`** - Contains module's input parameters.
- **`connection`** - Contains connection's data collection.
- **`common`** - Contains app's common data collection.
- **`data`** - Contains module's data collection.
- **`scenario`** - TBD
- **`metadata.expect`** - Contains module's raw parameters array the way you have specified it in the configuration.
- **`metadata.interface`** - Contains module's raw interface array the way you have specified it in the configuration.
- **`output`** - When using the `wrapper` directive, the `output` variable represents the result of the `output` directive

Additional variables available in triggers

- **`data.lastDate`** - Last processed item date. If you select to process all items in [Epoch Panel](epoch.html), this will be set to `1970-01-01` on the first run of the trigger.
- **`data.lastId`** - Last processed item id.
- **`trigger.matchEqual`** - if the user selected the first item from the [Epoch Panel](epoch.html), then this variable will be true. This is useful when the API allows you to filter based on date. In this case you would want to set the date filter to return items not strictly greater than `lastDate`, but greater or equal to `lastDate`.

Additional variables available to [Pagination Object](pagination-object.html) and [Response Object](response-object.html)

- **`body`** - Contains the body that was retrieved from the last request.
- **`headers`** - Contains the response headers that were retrieved from the last request.
- **`item`** - When iterating this variable represents the current item that is being iterated.

Additional variables available in [Attach](remote-procedures.html#attach) Remote Procedure.

- **`webhook.id`** - Internal webhook ID.
- **`webhook.url`** - Webhook URL, which you need to register in the remote service.

Additional variables available in [Detach](remote-procedures.html#detach) Remote Procedure.

- **`webhook`** - Contains webhook's data collection.

{% endraw %}
