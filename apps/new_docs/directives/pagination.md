Many times it is required to paginate the results that the server
returns. Either it is to retrieve older items, or because the server is
not returning the amount of items you want.

Below is the list of all pagination directives. Almost all of them
override or merge with original request directives, if

| Key                                     | Type                                                             | Description                                                                      |
|:----------------------------------------|:-----------------------------------------------------------------|:---------------------------------------------------------------------------------|
| [`mergeWithParent`](#merge-with-parent) | [Boolean](types.md#boolean)                                      | Specifies the condition if to execute pagination or not                          |
| [**url**](#pagination-url)              | [IML String](types.md#iml-string)                                | Specifies the URL that should be called.                                         |
| [**method**](#pagination-method)        | [IML String](types.md#iml-string)                                | Specifies the HTTP method, that should be used when issuing a request.           |
| [**headers**](#pagination-headers)      | [IML Flat Object](types.md#iml-flat-object)                      | A single level (flat) collection, that specifies request headers.                |
| [**qs**](#pagination-qs)                | [IML Flat Object](types.md#iml-flat-object)                      | A single level (flat) collection that specifies request query string parameters. |
| [**body**](#pagination-body)            | Any [IML Type](types.md#iml-types)                               | Specifies a request body.                                                        |
| [**condition**](#pagination-condition)  | [IML String](types.md#iml-string) or [Boolean](types.md#boolean) | Determines if to execute current request or never.                               |

### Detailed pagination directive description

#### `mergeWithParent` {#merge-with-parent}

This directive specifies if to merge pagination parameters with the
original request parameters, or not. Default value is `true`, which
means that all of your pagination request parameters will be merged with
the original request parameters.

If the value is set to `false`, then no parameters will be merged, but
directly used to make a pagination request.

#### `url` {#pagination-url}

This directive specifies the URL that will be called when the pagination
request is executed. It will override the original request URL no matter
the value of `margeWithParent`.

#### `method` {#pagination-method}

This directive specifies the HTTP method to be used when executing the
pagination request. It will override the original request method no
matter the value of `margeWithParent`.

#### `headers` {#pagination-headers}

This directive specifies the request headers to be used when executing
the pagination request. It will merge with headers of the original
request, overriding headers with the same name, when `mergeWithParent`
is set to `true` (default).

#### `qs` {#pagination-qs}

This directive specifies the request query string parameters to be used
when executing the pagination request. It will merge with query string
parameters of the original request, overriding ones with the same name,
when `mergeWithParent` is set to `true` (default).

#### `body` {#pagination-body}

This directive specifies the request body when the request method is
anything but `GET` to be used when executing the pagination request. It
will override the original request body no matter the value of
`margeWithParent`.

#### `condition` {#pagination-condition}

This directive specifies whether to execute the pagination request or
not. It has all context variables available to it as `response`
collection, such as `body`, `headers`, `temp` etc. Use this to stop
paginating if the server is sending you info about the amount of pages,
or items, or if a the next page is available or not.
