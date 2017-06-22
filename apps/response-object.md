---
title: Response Object
layout: apps
---

{% raw %}

Collection of directives controlling processing of response.

Key | Type | Description
--- | --- | ---
**type** | string | Specifies how data are parsed from body. Available values: `json` (default), `urlencoded`, `text` (or `string` or `raw`), `binary`.
**valid** | [IML](iml.html) | An expression that parses whether the response is valid or not. If not present, the response is considered always valid when the status code is between 200 and 399. If expression evaluates to a falsy value, the control is given to the `error` directive.
**limit** | [IML](iml.html) | Controls the maximum number of returned items by the module. Also used by [Pagination](pagination.html) to resolve how many pages needs to be queried. `1` by default for [Triggers](triggers.html), otherwise unlimited.
**error** | [IML](iml.html) | An expression that parses an error message from response body.
**error** | [Error Object](error-object.html) | Allow you to specify error types. Also allows you to react differently on various response status codes.
**iterate** | [IML](iml.html) | An expression that points to an array in response body. This allows you to use a special variable `item` that can be used to access properties of iterated items. Available only in [Triggers](triggers.html), [Searches](searches.html), [Remote Procedures](rpc.html) and [Actions](actions.html) (only when using the `wrapper` directive). **NOTE:** When using this directive in [Actions](actions.html), the `wrapper` directive must be specified and the Action must still return a single object and not an array. This is useful when you want to wrap multiple items in a single object and return it.
**iterate** | [Iterate Object](iterate-object.html) | Allows you to specify iteration condition.
**temp** | [Temp Object](temp-object.html) | Creates/updates variable `temp` which you can access in subsequent requests. It is not persistent and the data will be discarded once the execution is done.
**limit** | [IML](iml.html) or number | An expression that specifies the maximum number of items that will be returned by a module. `limit` is a global modifier, and as such only the `limit` which was specified in the last request will be used. Has no effect on Actions. Defaults to unlimited for Searches and RPCs. Defaults to 1 for Triggers.
**output** | [Output Object](output-object.html) | Describes structure of the output bundle.
**wrapper** | [Complex Object](complex-object.html) or Array<[Complex Object](complex-object.html)> | Use this object to wrap the module output, which will be available to you as the `output` context variable - the result of processing the `output` directive. When used, the value `wrapper` directive is what will become the final output of the module.

## Response Object Example

```json
{
    "response": {
        "valid": "{{body.success == true}}",
        "error": {
            "message": "{{body.message}}"
        },
        "iterate": "{{body.data.users}}",
        "output": {
            "id": "{{item.id}}",
            "fullName": "{{item.firstName + item.lastName}}"
        },
        "temp": {},
        "limit": "{{parameters.limit}}"
    }
}
```

See [Variables](variables.html)

{% endraw %}
