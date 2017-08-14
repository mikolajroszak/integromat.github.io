**Default**: `true`

If not present, the response is considered always valid when the status
code is between 200 and 399. If expression evaluates to a falsy value,
the control is given to the `error` directive.