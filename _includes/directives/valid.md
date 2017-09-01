**Required**: no  
**Default**: `true`

This directive lets you decide if the response returned by a service is
valid or not. Some services might return an HTTP status code >= 400 if
there was an error, but some might return < 400 status code and indicate
an error in the response body or headers. In the latter case it makes no
sense to output anything from the module, but instead indicate that
there was an error. That's when you use this directive.

If this directive is not present, the response is considered always
valid when the status code is between 200 and 399. If this directive
evaluates to a falsy (false, undefined, null etc.) value, the control is
given to the `error` directive. Otherwise the module will continue the
execution normally.