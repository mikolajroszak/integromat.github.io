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

- [Communication](#communication)
  - [Specification](#specification)
  - [Making requests](#making-requests)
    - [`url`](#url)
    - [`method`](#method)
    - [`headers`](#headers)
    - [`qs`](#qs)
    - [`body`](#body)
    - [`type`](#request-type)
    - [`temp`](#request-temp)
    - [`condition`](#condition)
    - [`followRedirects`](#follow-redirects)
    - [`followAllRedirects`](#follow-all-redirects)
  - [Multiple requests](#multiple-requests)
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
- [Parameters](#parameters)
- [Interface](#interface)

# Communication

## Specification

```text
{
    "url": String,
    "method": Enum[GET, POST, PUT, DELETE, OPTIONS],
    "qs": Flat Object,
    "headers": Flat Object,
    "body": Object|String|Array,
    "type": Enum[json, urlencoded, multipart/form-data, binary, text, string, raw],
    "ca": String,
    "condition": String|Boolean,
    "temp": Object,
    "oauth": { // available only when using OAuth 1 connection
        "consumer_key": String,
        "consumer_secret": String,
        "private_key": String,
        "token": String,
        "token_secret": String,
        "verifier": String,
        "signature_method": String,
        "transport_method": String,
        "body_hash": String
    },
    "response": {
        "type": Enum[json, urlencoded, xml, text, string, raw, binary, automatic]
        or
        "type": {
            "*": Enum[json, urlencoded, xml, text, string, raw, binary, automatic],
            "[Number[-Number]]": Enum[json, urlencoded, xml, text, string, raw, binary, automatic]
        },
        "temp": Object,
        "iterate": String,
        or
        "iterate": {
            "container": String|Array,
            "condition": String|Boolean
        },
        "output": String|Object|Array,
        "wrapper": String|Object|Array,
        "valid": String|Boolean,
        "error": String,
        or
        "error": {
            "message": String,
            "type": Enum[RuntimeError, DataError, RateLimitError, OutOfSpaceError, ConnectionError, InvalidConfigurationError, InvalidAccessTokenError, IncompleteDataError, DuplicateDataError],
            "[Number]": {
                "message": String,
                "type": Enum[RuntimeError, DataError, RateLimitError, OutOfSpaceError, ConnectionError, InvalidConfigurationError, InvalidAccessTokenError, IncompleteDataError, DuplicateDataError]
            }
        }
    },
    "pagination": {
        "mergeWithParent": Boolean,
        "url": String,
        "method": Enum[GET, POST, PUT, DELETE, OPTIONS],
        "headers": Flat Object,
        "qs": Flat Object,
        "body": Object|String|Array
    }
}
```

## Making Requests

In order to make a request you have to specify at least a `url`.
All other directives are not required.

All Available request-related directives are shown in the table below:

| Key                                               | Type                                                                               | Description                                                                      |
| :------------------------------------             | :-----------------------------------------------------------------                 | :------------------------------------------------------------------------------- |
| [**`url`**](#url)                                 | [IML String](articles/types.md#iml-string)                                         | Specifies the URL that should be called.                                         |
| [**`method`**](#method)                           | [IML String](articles/types.md#iml-string)                                         | Specifies the HTTP method, that should be used when issuing a request.           |
| [**`headers`**](#headers)                         | [IML Flat Object](articles/types.md#iml-flat-object)                               | A single level (flat) collection, that specifies request headers.                |
| [**`qs`**](#qs)                                   | [IML Flat Object](articles/types.md#iml-flat-object)                               | A single level (flat) collection that specifies request query string parameters. |
| **`ca`**                                          | [IML String](articles/types.md#iml-string)                                         | Custom Certificate Authority                                                     |
| [**`body`**](#body)                               | Any [IML Type](articles/types.md#iml-types)                                        | Specifies a request body.                                                        |
| [**`type`**](#request-type)                       | [String](articles/types.md#string)                                                 | Specifies how data are serialized into body.                                     |
| [**`temp`**](#request-temp)                       | [IML Object](articles/types.md#iml-object)                                         | Creates/updates the `temp` variable                                              |
| [**`condition`**](#condition)                     | [IML String](articles/types.md#iml-string) or [Boolean](articles/types.md#boolean) | Determines if to execute current request or never.                               |
| [**`followRedirects`**](#follow-redirects)        | [Boolean](articles/types.md#boolean)                                               | **Default:** true. Follow HTTP 3xx responses as redirects                        |
| [**`followAllRedirects`**](#follow-all-redirects) | [Boolean](articles/types.md#boolean)                                               | **Default:** true. Follow non-GET HTTP 3xx responses as redirects                |
| [**`response`**](#handling-responses)             | Response Specification                                                             | Collection of directives controlling processing of the response.                 |
| [**`pagination`**](#pagination)                   | Pagination Specification                                                           | Collection of directives controlling pagination logic.                           |


### `url`

{% include directives/url.md module="action" %}

### `method`

{% include directives/method.md %}

### `headers`

{% include directives/headers.md %}

### `qs`

{% include directives/qs.md %}

### `body`

{% include directives/body.md %}

### `type` {#request-type}

{% include directives/request.type.md %}

### `temp` {#request-temp}

{% include directives/request.temp.md %}

### `condition`

{% include directives/condition.md %}

### `followRedirects` {#follow-redirects}

{% include directives/followRedirects.md %}

### `followAllRedirects` {#follow-all-redirects}

{% include directives/followAllRedirects.md %}

## Multiple Requests

{% include sections/multiple_requests.md %}

## Handling responses

By default the module will output whatever it got from the remote
server.

Below is the collection of directives controlling processing of the
response. All of them must be placed inside the `response` collection.

| Key                          | Type                                                                | Description                                                                     |
| :--------------------------- | :---------------------------------------------------------------    | :------------------------------------------------------------------------------ |
| [**`type`**](#response-type) | [String](articles/types.md#string) or Type Specification            | Specifies how data are parsed from body.                                        |
| [**`valid`**](#valid)        | [IML String](articles/types.md#iml-string)                          | An expression that parses whether the response is valid or not.                 |
| [**`error`**](#error)        | [IML String](articles/types.md#iml-string) or Error Specification   | Specifies how the error is shown to the user, if it would occur.                |
| [**`iterate`**](#iterate)    | [IML String](articles/types.md#iml-string) or Iterate Specification | Specifies how response items (in case of multiple) are retrieved and processed. |
| [**`temp`**](#response-temp) | [IML Object](articles/types.md#iml-object)                          | Creates/updates variable `temp` which you can access in subsequent requests.    |
| [**`output`**](#output)      | Any [IML Type](articles/types.md#iml-types)                         | Describes structure of the output bundle.                                       |
| [**`wrapper`**](#wrapper)    | Any [IML Type](articles/types.md#iml-types)                         | Describes structure of the output bundle.                                       |

### `type` {#response-type}

{% include directives/response.type.md %}

### `valid`

{% include directives/valid.md %}

### `error`

{% include directives/error.md %}

### `iterate`

{% include directives/iterate.md module="action" %}

### `temp` {#response-temp}

{% include directives/response.temp.md %}

### `output`

{% include directives/output.md %}

### `wrapper`

{% include directives/wrapper.md %}

## Pagination (`pagination` directive) {#pagination}

{% include directives/pagination.md %}

## Request-less/Static mode {#static-mode}

{% include sections/static_mode.md %}

## IML variables

{% include sections/iml-variables.md module="action" %}

## Error handling

{% include sections/error-handling.md %}

# Parameters

{% include sections/parameters.md %}

# Interface

Describes structure of output bundles. Specification is the same as
[Parameters](#parameters).

```json
[
    {
        "name": "id",
        "type": "uinteger",
        "label": "User ID"
    }
]
```