---
title: Custom IML Functions
layout: apps
---



**IMPORTANT:** IML functions are disabled by default. If you need to
write you own function, please, contact us.

**NOTE:** Please read about
[Integromat Markup Language](iml.md) first.

IML Functions is a powerful feature, that allows you to write your own
JavaScript functions and execute them inside IML expressions to process
data.

All built-in IML functions + your custom IML functions are available for
you to use inside you own custom functions. You can access them in the
`iml` namespace like this: `iml.parseDate()`. List of built-in IML
functions is available
[here](https://www.integromat.com/en/kb/functions.html).

Only JavaScript built-in objects + Buffer are available for you to use.
You can use all features of ES 6, like arrow functions, destructuring,
etc...

## Example
{% raw %}
Here we implement a function in JavaScript.

```javascript
function processItem(item) {
    // You can do any sorts of computation here and then return the result
    return {
        created: iml.parseDate(item.created_at),
        username: item.username.toLowerCase(),
        fullName: `${item.firstName} ${item.lastName}`
    }
}
```

Now we are able to use it in IML:

```json
{
    "url": "/users",
    "response": {
        "output": "{{processItem(item)}}"
    }
}
```
{% endraw %}