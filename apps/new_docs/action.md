---
title: Action Module - Reference Documentation
layout: apps
---

The Action Module is a simple module that makes a request (or several)
and returns a result. It doesn't have state nor any internal complex
logic.

Use this module when you need to fire a one-shot action against a
service, for example to retrieve, update, or delete an item.

# Index

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
  - [`error`](#error)
    - [`message`](#error-message)
    - [`type`](#error-type)
    - [`\<status-code>`](#error-status-code)
  - [`iterate`](#iterate)
    - [`container`](#iterate-container)
    - [`condition`](#iterate-condition)
  - [`temp`](#response-temp)
  - [`output`](#output)
  - [`wrapper`](#wrapper)
- [Pagination](#pagination)
  - [`mergeWithParent`](#pagination-merge-with-parent)
  - [`url`](#pagination-url)
  - [`method`](#pagination-method)
  - [`headers`](#pagination-headers)
  - [`qs`](#pagination-qs)
  - [`body`](#pagination-body)
- [Request-less/Static mode](#static-mode)
- [IML variables](#iml-variables)
- [Error handling](#error-handling)

# Specification

```
{
    "url",
    "method",
    "qs",
    "headers",
    "body",
    "type",
    "ca",
    "condition",
    "response": {
        "type",
        "type": {
            "*",
            "[Number[-Number]]"
        },
        "temp",
        "iterate",
        "iterate": {
            "container",
            "filter"
        },
        "output",
        "wrapper",
        "valid",
        "error",
        "error": {
            "message",
            "type",
            "[Number]": {
                "message",
                "type"
            }
        }
    }
}
```
You can make multiple requests by placing above objects in an array.

# Making requests

In order to make a request you have to specify at least a `url`.
All other directives are not required.

All Available request-related directives are shown in the table below:

| Key                                   | Type                                                                         | Description                                                                      |
| :------------------------------------ | :-----------------------------------------------------------------           | :------------------------------------------------------------------------------- |
| [**url**](#url)                       | [IML String](other/types.md#iml-string)                                      | Specifies the URL that should be called.                                         |
| [**method**](#method)                 | [IML String](other/types.md#iml-string)                                      | Specifies the HTTP method, that should be used when issuing a request.           |
| [**headers**](#headers)               | [IML Flat Object](other/types.md#iml-flat-object)                            | A single level (flat) collection, that specifies request headers.                |
| [**qs**](#qs)                         | [IML Flat Object](other/types.md#iml-flat-object)                            | A single level (flat) collection that specifies request query string parameters. |
| **ca**                                | [IML String](other/types.md#iml-string)                                      | Custom Certificate Authority                                                     |
| [**body**](#body)                     | Any [IML Type](other/types.md#iml-types)                                     | Specifies a request body.                                                        |
| [**type**](#request-type)             | [String](other/types.md#string)                                              | Specifies how data are serialized into body.                                     |
| [**temp**](#request-temp)             | [IML Object](other/types.md#iml-object)                                      | Creates/updates the `temp` variable                                              |
| [**condition**](#condition)           | [IML String](other/types.md#iml-string) or [Boolean](other/types.md#boolean) | Determines if to execute current request or never.                               |
| [**response**](#handling-responses)   | Response Specification                                                       | Collection of directives controlling processing of the response.                 |
| [**pagination**](#pagination)         | Pagination Specification                                                     | Collection of directives controlling pagination logic.                           |

## Multiple Requests

{% include_relative sections/multiple_requests.md %}

## Detailed request directive description

### `url`

{% include_relative directives/url.md module="action" %}

### `method`

{% include_relative directives/method.md %}

### `headers`

{% include_relative directives/headers.md %}

### `qs`

{% include_relative directives/qs.md %}

### `body`

{% include_relative directives/body.md %}

### `type` {#request-type}

{% include_relative directives/request.type.md %}

### `temp` {#request-temp}

{% include_relative directives/request.temp.md %}

### `condition`

{% include_relative directives/condition.md %}

# Handling responses

By default the module will output whatever it got from the remote
server.

Below is the collection of directives controlling processing of the
response. All of them must be placed inside the `response` collection.

| Key                          | Type                                                             | Description                                                                     |
| :--------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------------------------------ |
| [**type**](#response-type)   | [String](other/types.md#string) or Type Specification            | Specifies how data are parsed from body.                                        |
| [**valid**](#valid)          | [IML String](other/types.md#iml-string)                          | An expression that parses whether the response is valid or not.                 |
| [**error**](#error)          | [IML String](other/types.md#iml-string) or Error Specification   | Specifies how the error is shown to the user, if it would occur.                |
| [**iterate**](#iterate)      | [IML String](other/types.md#iml-string) or Iterate Specification | Specifies how response items (in case of multiple) are retrieved and processed. |
| [**temp**](#response-temp)   | [IML Object](other/types.md#iml-object)                          | Creates/updates variable `temp` which you can access in subsequent requests.    |
| [**output**](#output)        | Any [IML Type](other/types.md#iml-types)                         | Describes structure of the output bundle.                                       |
| [**wrapper**](#wrapper)      | Any [IML Type](other/types.md#iml-types)                         | Describes structure of the output bundle.                                       |

## Detailed response directive description

### `type` {#response-type}

{% include_relative directives/response.type.md %}

### `valid`

{% include_relative directives/valid.md %}

### `error`

{% include_relative directives/error.md %}

### `iterate`

{% include_relative directives/iterate.md module="action" %}

### `temp` {#response-temp}

{% include_relative directives/response.temp.md %}

### `output`

{% include_relative directives/output.md %}

### `wrapper`

{% include_relative directives/wrapper.md %}

# Pagination (`pagination` directive) {#pagination}

{% include_relative directives/pagination.md %}

# Request-less/Static mode {#static-mode}

{% include_relative sections/static_mode.md %}

# IML variables

{% include_relative sections/iml-variables.md module="action" %}

# Error handling

{% include_relative sections/error-handling.md %}
