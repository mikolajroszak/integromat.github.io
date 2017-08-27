**Required**: no  
**Default**: `output`

This directive lets you post process module output before returning it
to the user. The output of the module will be available to you as the
`output` context variable - the result of processing the `output`
directive. When used, the value of the `wrapper` directive is what will
become the final output of the module. This directive is executed only
once and at the end of the processing chain. There are no more
directives or transformations after it.