---
title: Search Module - Reference Documentation
layout: apps
---

The Search Module is a module that makes a request (or several) and
returns multiple results. It doesn't have state nor any internal complex
logic.

Use this module when you need to allow the user to search for items or
to simply return multiple items.

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
  - [`limit`](#limit)
  - [`error`](#error)
    - [`message`](#error-message)
    - [`type`](#error-type)
    - [`\<status-code>`](#error-status-code)
  - [`iterate`](#iterate)
    - [`container`](#iterate-container)
    - [`condition`](#iterate-condition)
  - [`temp`](#response-temp)
  - [`output`](#output)
- [Pagination](#pagination)
  - [`mergeWithParent`](#pagination-merge-with-parent)
  - [`url`](#pagination-url)
  - [`method`](#pagination-method)
  - [`headers`](#pagination-headers)
  - [`qs`](#pagination-qs)
  - [`body`](#pagination-body)
- [Request-less/Static mode](#static-mode)

# Making requests

In order to make the simplest request, the only thing you have to
specify is a URL in the `url` directive. You can then specify the
request method via the `method` directive, add query string parameters
in the `qs` directive and headers in `headers` directive.

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

| Key                          | Type                                                                       | Description                                                                     |
| :--------------------------- | :---------------------------------------------------------------           | :------------------------------------------------------------------------------ |
| [**type**](#response-type)   | [String](other/types.md#string) or Type Specification                      | Specifies how data are parsed from body.                                        |
| [**valid**](#valid)          | [IML String](other/types.md#iml-string)                                    | An expression that parses whether the response is valid or not.                 |
| [**limit**](#limit)          | [IML String](other/types.md#iml-string) or [Number](other/types.md#number) | Controls the maximum number of returned items by the module.                    |
| [**error**](#error)          | [IML String](other/types.md#iml-string) or Error Specification             | Specifies how the error is shown to the user, if it would occur.                |
| [**iterate**](#iterate)      | [IML String](other/types.md#iml-string) or Iterate Specification           | Specifies how response items (in case of multiple) are retrieved and processed. |
| [**temp**](#response-temp)   | [IML Object](other/types.md#iml-object)                                    | Creates/updates variable `temp` which you can access in subsequent requests.    |
| [**output**](#output)        | Any [IML Type](other/types.md#iml-types)                                   | Describes structure of the output bundle.                                       |

## Detailed response directive description

### `type` {#response-type}

{% include_relative directives/response.type.md %}

### `valid`

{% include_relative directives/valid.md %}

### `limit`

{% include_relative directives/limit.md module="action" %}

### `error`

{% include_relative directives/error.md %}

### `iterate`

{% include_relative directives/iterate.md module="action" %}

### `temp` {#response-temp}

{% include_relative directives/response.temp.md %}

### `output`

{% include_relative directives/output.md %}

# Pagination (`pagination` directive) {#pagination}

{% include_relative directives/pagination.md %}

# Request-less/Static mode {#static-mode}

{% include_relative sections/static_mode.md %}

