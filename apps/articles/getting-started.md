---
title: Getting started
layout: apps
---


## Creating you first App

If you have searched through all the [Apps & Services](/en/integrations){:target="_blank"} that Integromat offers and did not find the one you would like to integrate in your scenario, you are in the right place. This DIY guide will help you to create the App yourself without writing a piece of code. All you need is to put together a few declarations in a very simple JSON format.

In the left main menu choose `My Apps`. The (probably still empty) list of all your Apps will appear. Click the button `Create a new App` in the right top corner. A dialog will pop up, where you can set some basic properties of your new App like its name etc. For the moment leave default values and click `Save`. Your new App will be shown in the list.

Click on your new App. A page with five tabs will appear: `Base`, `Connections`, `Webhooks`, `Modules` and `Remote Procedures`.

If your endpoint returns a single response, you have to use the
[Action](actions.html). Otherwise, if your endpoint returns multiple
items, you have to use the [Search](searches.html).

In this example we will start with a simple [Action](actions.html), that
returns a single item.

## Making a request to a service and retrieving a response

In order to make a simplest request to a remote server, only 1 parameter
is enough: the `url`. Suppose you have a web service, that allows to
retrieve information about the API on the `/info` path. The web service
is available on `http://yourservice.com` and it's API is available on
sub-path `/api`.

```json
{
    "url": "http://yourservice.com/api/info"
}
```

This will issue a request to `http://yourservice.com/api/info` and
return the entire body, that was returned.

## Customizing a request

Now, suppose that you want to retrieve a user. In order to do that, you
need to call the `/users/:id` method and specify the ID of the user you
want to retrieve. Here is how you would do that:

```json
{
    "url": "http://yourservice.com/api/users/{{parameters.userId}}"
}
```

To let user choose the parameter, add this to parameters:

```json
{
    "name": "userId",
    "type": "uinteger",
    "label": "User ID",
    "required": true
}
```

### Query string

If you need to specify a query string parameter, you can do:

```json
{
    "url": "http://yourservice.com/api/users/{{parameters.userId}}?includeAdvanced={{parameters.advanced}}"
}
```

But a better way is to use a special `qs` collection.

The `headers`, `qs` and `body` collections represent request headers,
query string parameters and body payload. The key is the variable/header
name and the value is the variable/header value. You don't need to
escape values inside those collections.

The above request can be rewritten as:

```json
{
    "url": "http://yourservice.com/api/users/{{parameters.userId}}",
    "qs": {
        "includeAdvanced": "{{parameters.advanced}}"
    }
}
```

### Headers

Now, lets say that your server wants only logged in users to view all
other users and it wants an `API-TOKEN` header with a correct value. We
can use the `headers` collection to specify this header:

```json
{
    "url": "http://yourservice.com/api/users/{{parameters.userId}}",
    "qs": {
        "includeAdvanced": "{{parameters.advanced}}"
    },
    "headers": {
        "API-TOKEN": "some-static-token"
    }
}
```

Cool, now we can make a request to a protected endpoint and retrieve a
list of users.

**NOTE**: `qs` and `headers` are single level collections, meaning that
you cannot specify nested objects in their parameters:

```json
{
    "qs": {
        "someProp": {
            "anotherOne": {
                "and-one-more": "THIS WILL NOT WORK"
            }
        }
    }
}
```

The example above will not work. But if you want dot notation for some
reason, you can use it directly in the parameter name:

```json
{
    "qs": {
        "someProp.anotherOne.and-one-more": "THIS WILL WORK"
    }
}
```

This will create a query string that looks like this:
`?someProp.anotherOne.and-one-more=THIS%20WILL%20WORK`

## Retrieving an array of items

Suppose you want to retrieve all users, that are registered on your
service. You can't use [Action](actions.html), because it returns only
single result. You will have to create a [Search](searches.html) module
for this.

The communication for [Search](searches.html) is the same as for
`Action`, except `Search` has an [`iterate`](response-object.html)
directive, that specifies where are the items located inside the body.

For the next example, suppose that when you call `/users` on your
service, you will get a list of users in `body.data`.

```json
{
    "url": "http://yourservice.com/api/users",
    "qs": {
        "includeAdvanced": "{{parameters.advanced}}"
    },
    "headers": {
        "API-TOKEN": "some-static-token"
    },
    "response": {
        "iterate": "{{body.data}}"
    }
}
```

This example will correctly output each user that was returned. But what
if you don't want to output all parameters? You can use the
[`output`](response-object.html) section to manually map response to
module's output:

```json
{
    "url": "http://yourservice.com/api/users",
    "qs": {
        "includeAdvanced": "{{parameters.advanced}}"
    },
    "headers": {
        "API-TOKEN": "some-static-token"
    },
    "response": {
        "iterate": "{{body.data}}",
        "output": {
            "id": "{{item.id}}",
            "username": "{{item.username}}"
        }
    }
}
```

## Creating a new Connection

We have covered the basics about creating a simple module. Now, let's
see how to update our search module with a variable API token for each
user. Create a new [Connection](basic-connection.html) and choose **API
Key** as a connection type. A pre-configured communication will look
like this:

```json
{
    "qs": {},
    "url": "https://www.example.com/api/whoami",
    "body": {},
    "method": "GET",
    "headers": {
        "API-TOKEN": "{{parameters.apiKey}}"
    }
}
```

This section specifies a simple request to determine whether the
credentials entered by the user are valid or not. The most common way to
validate the credentials is to call an API to get user's information.
Most of the APIs have such an API.

Once you finish communication configuration, you can go back to your
Search module and click *Attach connection*. Once you select a
connection, you can update your Search's communication headers to
execute request with variable API token like this:

```json
{
    "headers": {
        "API-TOKEN": "{{connection.apiKey}}"
    }
}
```

