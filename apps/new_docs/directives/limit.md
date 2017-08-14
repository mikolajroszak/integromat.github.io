{% case include.module %}
  {% when "trigger" %}
    {% assign amount = "1" %}
  {% else %}
    {% assign amount = "unlimited" %}
{% endcase %}

**Default**: {{amount}}

This directive specifies the maximum items that will be returned by the
module. Note that in multi-request configuration only limit of last
request will be used, even if it is not specified on it, because limit
has a default value.

The `limit` directive is also used by pagination logic to determine
whether to fetch the next page or no.

The **default** limit is for this type of module is **{{amount}}**.

