---
title: Pagination
layout: apps
---

{% raw %}

The `pagination` property inside [Request Object](request-object.html) specifies how to get the next batch of items, if the endpoint is using pagination.

```json
{
    "url": "/posts",
    "method": "GET",
    "headers": {},
    "qs": {
        "skip": 0,
        "limit": 50
    },
    "body": {},
    "response": {
        "trigger": {
            "id": "{{item.id}}",
            "date": "{{item.createdAt}}",
            "type": "date",
            "order": "asc"
        },
        "limit": "{{parameters.limit}}",
        "iterate": "{{body.data.posts}}",
        "output": {
            "id": "{{item.id}}",
            "author": "{{item.author}}",
            "text": "{{item.text}}"
        },
        "temp": {
            "page": "{{ifempty(temp.page, 0) + 1}}"
        }
    },
    "pagination": {
        "condition": "{{if(body.pagination.nextPage, true, false)}}",
        "qs": {
            "skip": "{{temp.page * 50}}"
        }
    }
}
```

You can use the same `url`, `method`, `headers`, `qs` and `body` parameters described above, but they will act as 
overrides of the initial request by default. Please see `mergeWithParent` property if you want to create a blank pagination request.

There is no `response` directive in the pagination, because it is assumed that the response from the initial and pagination requests have the same structure. 
As such, the root `response` directive will be used to process both initial and pagination requests.

Normally you would not need to specify the `pagination.condition` directive, since the module will stop automatically when it reaches items which were already processed.
You might want to use it with APIs that return a flag in the body (or headers), indicating if the next page is available or not.

When specifying the `pagination` request, please select a reasonable number of items that an API can return in one request.

**Examples**:

If the API uses skip/limit or page/pageSize technique, you can implement it like in the example below, provided the API returns the current page (or next page) number. 

```json
{
    "url": "/posts",
    "qs": {
        "skip": 0,
        "limit": 50
    },
    "pagination": {
        "qs": {
            "skip": "{{(body.additional_data.page + 1) * 50}}"
        }
    }
}
```

If the API does not return the current page, you can create a `temp` variable called `page` (or any other name you like), and use it in the pagination request like so:

```json
{
    "response": {
        "temp": {
        "page": "{{ifempty(temp.page, 0) + 1}}" 
        }
    }
}
```

This will create a page variable with initial value of 1, and after each request it will increment this variable by 1.

```json
{
    "pagination": {
        "qs": {
            "skip": "{{temp.page * 50}}"
        }
    }
}
```

If the API sends the next page URL in the response body, you can use it directly like so:

```json
{
    "pagination": {
        "mergeWithParent": false,
        "url": "{{body.pagination.next_page}}"
    }
}
```

### `pagination.condition`

<table>
<tr><td><strong>Required</strong></td><td>false</td></tr>
<tr><td><strong>Default</strong></td><td>undefined</td></tr>
</table>

This property controls the pagination. If the expression specified in this property resolves in a falsy value, pagination will NOT be executed.

### `pagination.mergeWithParent`

<table>
<tr><td><strong>Required</strong></td><td>false</td></tr>
<tr><td><strong>Default</strong></td><td>true</td></tr>
</table>

This property specifies if the pagination request should be merged with the initial module request.
    
By default this is set to `true`. You can then override specific parts of `qs` or `headers` or other properties to fetch the next batch.

If you set this property to `false` - `url`, `headers`, `qs` and `body` parameters from the initial request will NOT be copied over to the pagination request, so you will have to provide the full request specification, including the URL.
        
**NOTE**: the `url` property will still be resolved against the `baseUrl` property, if it is a relative URL. If it is an absolute URL, on the other hand, it will be kept as-is.

{% endraw %}