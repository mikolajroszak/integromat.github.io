**Default**: `body`, when `iterate` is not set; `item` otherwise

With the `output` directive you can specify how you want your module
output to look. When using iterate, `output` is processed for each item
of the array, that was specified with the `iterate` directive. When not
using iterate, `output` is executed only once and the result is fed to
the `wrapper` directive, if present, otherwise it will be the final
result of the module.