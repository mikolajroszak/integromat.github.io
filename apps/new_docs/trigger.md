---
title: Actions
layout: apps
---

# Trigger Module - Reference Documentation

## Index

- [Making requests](#making-requests)
  - [`url`](#url)
  - [`method`](#method)
  - [`headers`](#headers)
  - [`qs`](#qs)
  - [`body`](#body)
  - [`type`](#request-type)
  - [`temp`](#request-temp)
  - [`condition`](#condition)
- [Handling responses](#handling-responses)
  - [`type`](#response-type)
  - [`valid`](#valid)
  - [`limit`](#limit)
  - [`error`](#error)
  - [`iterate`](#iterate)
  - [`temp`](#response-temp)
  - [`output`](#output)
  - [`wrapper`](#wrapper)
- [Pagination](#pagination)
  - [`mergeWithParent`](#merge-with-parent)
  - [`url`](#pagination-url)
  - [`method`](#pagination-method)
  - [`headers`](#pagination-headers)
  - [`qs`](#pagination-qs)
  - [`body`](#pagination-body)

## Making requests

In order to make the simplest request, the only thing you have to
specify is a URL in the `url` directive. You can then specify the
request method via the `method` directive, add query string parameters
in the `qs` directive and headers in `headers` directive

All Available request-related directives are shown in the table below:

| Key                                 | Type                                                             | Description                                                                      |
|:------------------------------------|:-----------------------------------------------------------------|:---------------------------------------------------------------------------------|
| [**url**](#url)                     | [IML String](types.md#iml-string)                                | Specifies the URL that should be called.                                         |
| [**method**](#method)               | [IML String](types.md#iml-string)                                | Specifies the HTTP method, that should be used when issuing a request.           |
| [**headers**](#headers)             | [IML Flat Object](types.md#iml-flat-object)                      | A single level (flat) collection, that specifies request headers.                |
| [**qs**](#qs)                       | [IML Flat Object](types.md#iml-flat-object)                      | A single level (flat) collection that specifies request query string parameters. |
| **ca**                              | [IML String](types.md#iml-string)                                | Custom Certificate Authority                                                     |
| [**body**](#body)                   | Any [IML Type](types.md#iml-types)                               | Specifies a request body.                                                        |
| [**type**](#request-type)           | [String](types.md#string)                                        | Specifies how data are serialized into body.                                     |
| [**temp**](#request-temp)           | [IML Object](types.md#iml-object)                                | Creates/updates the `temp` variable                                              |
| [**condition**](#condition)         | [IML String](types.md#iml-string) or [Boolean](types.md#boolean) | Determines if to execute current request or never.                               |
| [**response**](#handling-responses) | Response Specification                                           | Collection of directives controlling processing of the response.                 |
| [**pagination**](#pagination)       | Pagination Specification                                         | Collection of directives controlling pagination logic.                           |

### Detailed request directive description

#### `url`

{% include_relative directives/url.md %}

#### `method`

{% include_relative directives/method.md %}

#### `headers`

{% include_relative directives/headers.md %}

#### `qs`

{% include_relative directives/qs.md %}

#### `body`

{% include_relative directives/body.md %}

#### `type` {#request-type}

{% include_relative directives/request.type.md %}

#### `temp` {#request-temp}

{% include_relative directives/temp.md %}

#### `condition`

{% include_relative directives/condition.md %}

## Handling responses

By default the module will output whatever it got from the remote
server.

Below is the collection of directives controlling processing of the
response. All of them must be places inside the `response` collection.

| Key                        | Type                                                           | Description                                                                     |
|:---------------------------|:---------------------------------------------------------------|:--------------------------------------------------------------------------------|
| [**trigger**](#trigger)    | Trigger Specification                                          | Collection of directives controlling trigger logic                              |
| [**type**](#response-type) | [String](types.md#string) or Type Specification                | Specifies how data are parsed from body.                                        |
| [**valid**](#valid)        | [IML String](types.md#iml-string)                              | An expression that parses whether the response is valid or not.                 |
| [**limit**](#limit)        | [IML String](types.md#iml-string) or [Number](types.md#number) | Controls the maximum number of returned items by the module.                    |
| [**error**](#error)        | [IML String](types.md#iml-string) or Error Specification       | Specifies how the error is shown to the user, if it would occur.                |
| [**iterate**](#iterate)    | [IML String](types.md#iml-string) or Iterate Specification     | Specifies how response items (in case of multiple) are retrieved and processed. |
| [**temp**](#response-temp) | [IML Object](types.md#iml-object)                              | Creates/updates variable `temp` which you can access in subsequent requests.    |
| [**output**](#output)      | Any [IML Type](types.md#iml-types)                             | Describes structure of the output bundle.                                       |
| [**wrapper**](#wrapper)    | Any [IML Type](types.md#iml-types)                             | Use this object to wrap the module output.                                      |

### Detailed response directive description

#### `trigger`

The trigger collection specifies directives that will control how the
trigger will work and how your data will be processed

| Key                         | Type                              | Description                                                   |
|:----------------------------|:----------------------------------|:--------------------------------------------------------------|
| [**type**](#trigger-type)   | `date` or `id                     | Specifies how the trigger will behave and sort items          |
| [**order**](#trigger-order) | `asc` or `desc`                   | Specifies in what order the remote API returns items          |
| [**id**](#trigger-id)       | [IML String](types.md#iml-string) | Must return current item's Id                                 |
| [**date**](#trigger-date)   | [IML String](types.md#iml-string) | When used, must return current item's create/update timestamp |

#### `trigger.type` {#trigger-type}

The `trigger.type` directive specifies how the trigger will sort and
iterate through items.

If the processed item has a create/update timestamp, then `date` should
be used as a value, and a correct getter should be specified in
`trigger.date` directive. The trigger will then sort all items by their
date and id fields and return only unprocessed items.

If the processed item does not have a create/update timestamp, but only
an id, then `id` should be used as a value, and a correct getter should
be specified in `trigger.id` directive.

#### `trigger.order` {#trigger-order}

The `trigger.order` directive specifies in what order is the remote API
returning items - descending or ascending. This information is needed to
correctly determine if there are more pages to be fetched or no. It is
also needed to correctly sort the incoming items and display them to the
user in ascending order.

So if the API is returning items in ascending order (low to high), then
`asc` should be used. If the API is returning items in descending order
(high to low), then `desc` should be used.

#### `trigger.id` {#trigger-id}

This directive specifies the item's id. It must always be present. For
example, if your item looks like this

```json
{
    "id": 24,
    "name": "Fred",
    "friend_count": 5
}
```
{% raw %}
Then you should specify your `trigger.id` directive like this:
`{{item.id}}`

```json
{
    "response": {
        "trigger": {
            "id": "{{item.id}}"
        }
    }
}
```
{% endraw %}

#### `trigger.date` {#trigger-date}

This directive specifies the item's date. It must be specified when the
`trigger.type` is set to `date`. Note that `trigger.id` must always be
specified.

For example, if your item looks like this

```json
{
    "id": 24,
    "name": "Fred",
    "friend_count": 5,
    "created_date": "2017-07-05T13:05"
}
```

{% raw %}
Then you should specify your `trigger.date` directive like this:
`{{item.created_date}}`, and your trigger collection might look
something like this

```json
{
    "response": {
        "trigger": {
            "id": "{{item.id}}",
            "date": "{{item.created_date}}"
        }
    }
}
```
{% endraw %}

#### `type` {#response-type}

{% include_relative directives/response.type.md %}

#### `valid`

{% include_relative directives/valid.md %}

#### `limit`

{% include_relative directives/limit.md module="trigger" %}

#### `error`

{% include_relative directives/error.md %}

#### `iterate`

{% include_relative directives/iterate.md module="trigger" %}

#### `temp` {#response-temp}

{% include_relative directives/response.temp.md %}

#### `output`

{% include_relative directives/output.md %}

#### `wrapper`

{% include_relative directives/wrapper.md %}

## Pagination

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
