The IML variables are variables that you are able to use in IML
expressions.

These IML variables are available for you to use everywhere in this module:

- **`now`** - Current date and time
- **`environment`** - TBD
- **`temp`** - Contains custom variables created via `temp` directive
{% if include.module == "connection" %}
- **`parameters`** - Contains connection's input parameters.
- **`common`** - Contains connection's common data collection.
- **`data`** - Contains connections's data collection.
- **`oauth.scope`** - Contains an array of scope required to be passed
  to OAuth2 authorization process.
- **`oauth.redirectUri`** - Contains redirect URL for OAuth2
  authorization process.
{% if include.module == "connection" and type == "oauth2" %}
- **`query`** - Contains query parameters returned by a 3rd party during
  OAuth2 authorization.
{% endif %}
{% endif %}
{% if include.module == "action" or include.module == "rpc" or include.module == "search" or include.module == "trigger" %}
- **`parameters`** - Contains module's input parameters.
- **`connection`** - Contains connection's data collection.
- **`common`** - Contains app's common data collection.
- **`data`** - Contains module's data collection.
- **`scenario`** - TBD
- **`metadata.expect`** - Contains module's raw parameters array the way
  you have specified it in the configuration.
- **`metadata.interface`** - Contains module's raw interface array the
  way you have specified it in the configuration.
{% endif %}
{% if include.module == "trigger" %}
- **`data.lastDate`** - Last processed item date. If you select to
  process all items in Epoch Panel, this will be set to
  `1970-01-01` on the first run of the trigger.
- **`data.lastId`** - Last processed item id.
- **`trigger.matchEqual`** - if the user selected the first item from
  the Epoch Panel, then this variable will be true. This
  is useful when the API allows you to filter based on date. In this
  case you would want to set the date filter to return items not
  strictly greater than `lastDate`, but greater or equal to `lastDate`.
{% endif %}
{% if include.module == "action" or include.module == "rpc" or include.module == "search" or include.module == "trigger" %}
Additional variables available to Response Object:
- **`output`** - When using the `wrapper` directive, the `output`
  variable represents the result of the `output` directive

Additional variables available to Pagination and Response Objects:
- **`body`** - Contains the body that was retrieved from the last
  request.
- **`headers`** - Contains the response headers that were retrieved from
  the last request.
- **`item`** - When iterating this variable represents the current item
  that is being iterated.
{% endif %}
Additional variables available in [Attach](rpc.md#attach-rpc) Remote
Procedure.
- **`webhook.id`** - Internal webhook ID.
- **`webhook.url`** - Webhook URL, which you need to register in the
  remote service.

Additional variables available in [Detach](rpc.md#detach-rpc) Remote
Procedure.
- **`webhook`** - Contains webhook's data collection.
