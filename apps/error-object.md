---
title: Error Object
layout: apps
---

{% raw %}

Allow you to specify error types. Also allows you to react differently on various response status codes.

Key | Type | Description
--- | --- | ---
**value** | [IML](iml.html) | An expression that parses an error message from response body.
**type** | [IML](iml.html) | An expression that parses an error type from response body.

## Available Error Types

- **`RuntimeError`** - Primary error type. Execution is interrupted and rolled back.
- **`DataError`** - Incoming data are invalid. If incomplete executions are enabled, execution is interrupted and the state is stored. User is able to repair the data and resume execution.
- **`RateLimitError`** - Service responded with rate-limit related error. Applies delay to next execution of a scenario.
- **`OutOfSpaceError`** - User is out of space.
- **`ConnectionError`** - Connection related problem. Applies delay to next execution of a scenario.
- **`InvalidConfigurationError`** - Configuration related problem. Deactivates the scenario and notifies the user.
- **`InvalidAccessTokenError`** - Access token related problem. Deactivates the scenario and notifies the user.
- **`IncompleteDataError`** - Incoming data are incomplete.
- **`DuplicateDataError`** - Reports error as warning, does not interrupt execution. If incomplete executions are enabled, execution is interrupted and the state is stored. User is able to repair the data and resume execution.

## Error Object Example

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