**Required**: no  
**Default**: true

This directive controls the encoding of URLs. 
It is on by default, so if you have any special characters in your URL, they
will be automatically encoded. But there might be situations where you don't 
want your URL to be encoded automatically, or you want to control what parts
of the URL are encoded. To do this, set this flag to `false`. 

For example you might have a URL component that includes a `/` and it must be 
encoded as `%2F`, but Apps treat your slashes as URL path delimiters by default.
I.e. you need the URL to look like `https://example.com/Hello%2FWorld`, but 
instead it looks like `https://example.com/Hello/World`, which is a totally different
URL. 
So the obvious solution would be to just use `{{encodeURL(parameters.component)}}`. 
But when you do that, you will receive `%252F`, because you URL is encoded twice: 
once by you in your IML expression, and the second time by Apps. To disable this 
behaviour set this flag to `false`. 
