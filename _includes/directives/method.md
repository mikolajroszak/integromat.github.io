**Required**: no  
**Default**: `GET`  
**Values**: `GET`, `POST`, `PUT`, `DELETE` (and other HTTP methods)

This directive specifies the HTTP method that will be used to issue the
request. Possible values are `GET` (default), `POST`, `DELETE`, `PUT`,
etc. You can specify the method with an IML expression based on module
parameters, like so:
{% raw %}
```json
{
    "url": "http://example.com/entity",
    "method": "{{if(parameters.create, 'POST', 'PUT')}}" 
}
```
{% endraw %}