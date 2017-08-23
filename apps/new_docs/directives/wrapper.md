**Required**: no  
**Default**: `output`

You are able to post process module output, before returning it to the
user with the `wrapper` directive. The output of the module will be
available to you as the `output` context variable - the result of
processing the `output` directive. When used, the value of the `wrapper`
directive is what will become the final output of the module. This
directive is executed only once and the end of the processing chain.
There are no more transformation after it.