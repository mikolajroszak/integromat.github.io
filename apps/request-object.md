---
title: Request Object
layout: apps
---

{% raw %}

Collection of directives controlling processing of a request.

Key | Type | Description
--- | --- | ---
**url** | [IML](iml.html) | Specifies the URL that should be called. Just by specifying it you are able to call a remote endpoint and retrieve data. If this parameter is not present, only the data that is specified in `output` and/or `wrapper` will be returned and no request will be made (request-less/static mode).
**method** | string | Specifies the HTTP method, that should be used when issuing a request: `GET` (default), `POST`, `DELETE`, `PUT`, etc.
**headers** | [Flat Object](flat-object.html) | A single level (flat) collection, that specifies request headers.
**qs** | [Flat Object](flat-object.html) | A single level (flat) collection that specifies request query string parameters.
**ca** | [IML](iml.html) | Custom Certificate Authority
**body** | [Complex Object](complex-object.html) | Specifies a request body, when the `method` property is set to anything except `GET`. The value of this property can be an arbitrary JSON object or a primitive: a number, a boolean, a string, an object, an array. This property will undergo IML transformation, replacing IML expressions with values and the result will be send as the request payload.
**type** | string | Specifies how data are serialized into body. Available values: `json` (default), `urlencoded`, `multipart/form-data`, `text` (or `string` or `raw`), `binary`. **NOTE:** When using `text` (or `string` or `raw`) the body must be a string. **NOTE 2:** When using `json` or `multipart/form-data` or `urlencoded` the appropriate value of `Content-Type` header will be set automatically based on type.
**temp** | [Temp Object](temp-object.html) | Creates/updates variable `temp` which you can access in other blocks. It is not persistent and the data will be discarded once the execution is done.
**condition** | [IML](iml.html) or boolean | An expression that must evaluate to boolean (true or false). Determines if to execute current request or never.
**response** | [Response Object](response-object.html) | Collection of directives controlling processing of response.
**pagination** | [Pagination Object](pagination-object.html) | Collection of directives controlling pagination logic.

### `multipart/form-data` type

#### Objects

Objects are not supported by `multipart/form-data`. All objects, except the special object to define files (see below), will be removed. `null` and `undefined` properties will be removed as well.

#### Booleans and Numbers

Booleans and numbers will be automatically converted to strings. Numbers are converted as usual: `1 -> "1"` and booleans will be converted to lowercase strings: `true -> "true"`, `false -> "false"`

#### Files

When using `multipart/form-data` you can add files in two ways: just specifying the buffer or specifying the buffer with additional info.

##### Option 1:
When specifying just the buffer with the file you can use normal notation:

```json
{
    "url": "/file",
    "method": "POST",
    "type": "multipart/form-data",
    "body": {
        "data": "{{parameters.file}}"
    }
}
```

This will successfully post a file, which is contained in the `parameters.file` Buffer, within the `data` part to the remote server.

##### Option 2:
You can use additional `options` object to specify file name and file MIME type. There are only 2 options available: `filename` and `contentType`.
Note that you have to specify the file in the `value` key and when using this notation you **have** to specify options in the `options` key, just like it is shown below:

```json
{
    "url": "/file",
    "method": "POST",
    "type": "multipart/form-data",
    "body": {
        "data": {
            "value": "{{parameters.file}}",
            "options": {
                "filename": "file.txt",
                "contentType": "text/plain"
            }
        }
    }
}
```

This will do the same thing as Option 1 (POST the file withing the `data` part), but it will also specify the file name and file MIME type.

## Returning static content (request-less/static mode)

If you want to just remove some static data, you must remove the `url` directive from your request specification. This will simply return data that is specified in `output` and/or `wrapper` and no request will be made to a remote server.

## Request Object Example

Below is a quick example of a module, that has only 1 request, that paginates.

```json
{
    "url": "/posts",
    "method": "POST",
    "temp": {
        "radius": 3.14
    },
    "headers": {
        "TOKEN": "{{connection.token}}",
        "X-SOME-OTHER-HEADER": "some other value; and another one"
    },
    "qs": {
        "qsParam": "value"
    },
    "body": {
        "radius": "{{temp.radius}}",
        "diameter": "{{2 * temp.radius}}",
        "circumference": "{{2 * pi * temp.radius}}"
    },
    "response": {
        "output": {
            "userId": "{{body.userId}}",
            "userName": "{{body.userName}}",
            "signature": "{{body.signature}}"
        },
        "limit": "{{parameters.limit}}"
    },
    "pagination": {
        "condition": "{{body.next == true}}",
        "mergeWithParent": false,
        "qs": {
            "skip": "50"
        }
    }
}
```

See [Variables](variables.html)

Specifying multiple requests is as simple as wrapping many single request specification items inside an array.

```javascript
[
    {/* ...single request specification... */},
    {/* ...another single request specification... */},
    {/* ...and another one... */}
]
```

The requests are executed in serial. The only way to retain information between requests is to set your own variables in `temp` (see [Response Object](response-object.md)), which is shared between all requests.

**NOTE**: The module returns `output` data only from the last request.

{% endraw %}
