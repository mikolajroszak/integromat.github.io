**Required**: {% if include.module == "trigger" %}yes{% else %}no{% endif %}  
**Default**: empty

This directive specifies the request URL.
{% if include.module == "trigger" %}
It must be always present. The Trigger does not support
request-less/static mode, because it makes no sense for this particular
module.
{% else %}
If this directive is not present, the module will work in
request-less/static mode, which means that it will simply return the
data specified in `response.output`.
{% endif %}
