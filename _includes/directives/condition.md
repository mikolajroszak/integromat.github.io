**Required**: no  
**Default**: true

This directive specifies whether to execute the request or not.

If this directive is not specified - the request will always be
executed.

If the value of this directive is `false`, then the request will not be
executed, and the flow will go to next request, if present, or return
nothing.
