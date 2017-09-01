---
title: Base
layout: apps
---

{% raw %}

## Specification

Collection of parameters all modules and remote procedures inherits
from. It is useful e.g. for storing a base URL and authorization
headers.

```
{
    "baseUrl",
    "headers",
    "qs",
    "body",
    "oauth"
}
```

| Key         | Type                                        | Description                                                                                                 |
| ---         | ---                                         | ---                                                                                                         |
| **baseUrl** | [String](../articles/types.md#string)                   | If you want to use this base URL in a request, you need to start the URL of an endpoint with `/` character. |
| **headers** | [IML Flat Object](../articles/types.md#iml-flat-object) | Default headers that every module will use.                                                                 |
| **qs**      | [IML Flat Object](../articles/types.md#iml-flat-object) | Default query string parameters that every module will use.                                                 |
| **body**    | [IML Object](../articles/types.md#iml-object)           | Default request body that every module will use when issuing a POST or PUT request.                         |
| **oauth**   | Oauth 1 Parameter Specification             | Collection of directives containing parameters for the OAuth 1 protocol.                                    |

{% endraw %}