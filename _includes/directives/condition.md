**Required**: no  
**Default**: `true`

This directive specifies whether to execute the request or not.

If this directive is not specified - the request will always be
executed.

If the value of this directive is `false`, then the request will not be
executed, and the flow will go to next request, if present, or return
nothing.

When you need to return some data when the condition is false, you are able to
specify the `condition` directive as an object, in which case it will have
the following properties:

| Key                                     | Type                                        | Description                                             |
| ---                                     | ---                                         | ---                                                     |
| [**`condition`**](#condition-condition) | [IML String](types.md#iml-string)           | Specifies if to execute the request or not              |
| [**`default`**](#condition-default)     | Any [IML Type](articles/types.md#iml-types) | Specifies the module output when the condition is false |

### `condition.condition` {#condition-condition}

**Required**: no  
**Default**: empty

The original `condition` directive. If the condition is not specified, the request is always executed.

### `condition.default` {#condition-default}

**Required**: no  
**Default**: empty

An optional module output when the condition is false.
