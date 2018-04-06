**Required**: no  
**Default**: empty

The `error` directive specifies the error type and the error message to
show the user. In it's simplest form, it only specifies the error
message:

{% raw %}
```json
{
    "response": {
        "error": "{{body.error.message}}"
    }
}
```
{% endraw %}

The message contained in `body.error.message` will then be shown to the
user, if the request will fail with an error, or a response with status
code >= 400 will be received.

You are also able to to specify different error messages based on
different status codes. The error object has the following attributes:
{% raw %}
| Key                                      | Type                              | Description                                                                    |
| ---                                      | ---                               | ---                                                                            |
| [**message**](#error-message)            | [IML String](types.md#iml-string) | An expression that parses an error message from response body.                 |
| [**type**](#error-type)                  | [IML String](types.md#iml-string) | An expression that specifies the error type.                                   |
| [**\<status code>**](#error-status-code) | Error Specification               | An object that customizes the error message and type based on the status code. |
{% endraw %}

### `error.message` {#error-message}
**Required**: yes  
**Default**: empty

The `error.message` directive specifies the message that the error will
contain. It can be a statically specified string, or it can point to a
message in, for example, response body or headers.

### `error.type` {#error-type}
**Required**: no  
**Default**: RuntimeError

The `error.type` directive specifies a type of the error message.
Different error types are handled differently by Integromat. The default
error type is `RuntimeError`. You can read about all available error
types in the list below.

**Available Error Types**

- **`RuntimeError`** - Primary error type. Execution is interrupted and
  rolled back.
- **`DataError`** - Incoming data is invalid. If incomplete executions
  are enabled, execution is interrupted and the state is stored. User is
  able to repair the data and resume execution.
- **`RateLimitError`** - Service responded with rate-limit related
  error. Applies delay to next execution of a scenario.
- **`OutOfSpaceError`** - User is out of space.
- **`ConnectionError`** - Connection related problem. Applies delay to
  next execution of a scenario.
- **`InvalidConfigurationError`** - Configuration related problem.
  Deactivates the scenario and notifies the user.
- **`InvalidAccessTokenError`** - Access token related problem.
  Deactivates the scenario and notifies the user.
- **`IncompleteDataError`** - Incoming data is incomplete.
- **`DuplicateDataError`** - Reports error as warning, does not
  interrupt execution. If incomplete executions are enabled, execution
  is interrupted and the state is stored. User is able to repair the
  data and resume execution.

### `error.<status-code>` {#error-status-code}

**Required**: no  
**Default**: empty

You are able to specify custom errors for different status codes by
specifying the status code as the key in the `error` directive object,
and using the same error specification as a value. See examples below.

**Examples**:

Here is a simple example of an error message with type
{% raw %}
```json
{
    "response": {
        "error": {
            "type": "DataError",
            "message": "{{body.message}}"
        }
    }
}
```
{% endraw %}
This will output an error of type `DataError` with message contained in
`body.message`

A more complex example including multiple status codes:
```json
{
    "response": {
        "error": {
            "type": "RuntimeError",
            "message": "Generic error message",
            "400": {
                "type": "DataError",
                "message": "Your request was invalid"
            },
            "500": {
                "type": "ConnectionError",
                "message": "The server was not able to handle your request"
            }
        }
    }
}
```

