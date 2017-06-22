---
title: Base
layout: apps
---

{% raw %}

## Specification

Collection of parameters all modules and remote procedures inherits from. It is useful e.g. for storing a base URL and authorization headers.

```json
{
    "baseUrl": "https://www.example.com",
    "headers": {
        "x-api-key": "{{connection.apiKey}}"
    }
}
```

Key | Type | Description
--- | --- | ---
**baseUrl** | string | If you want to use this base URL in a request, you need to start the URL of an endpoint with `/` character.
**headers** | [Flat Object](flat-object.html) | Default headers that every module will use.
**qs** | [Flat Object](flat-object.html) | Default query string parameters that every module will use.
**body** | [Complex Object](complex-object.html) | Default request body that every module will use when issuing a POST or PUT request.

See [Request Object](request-object.html), [Variables](variables.html)

{% endraw %}