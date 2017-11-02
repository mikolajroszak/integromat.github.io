---
title: Getting started
layout: apps
---

## Creating you first App

If you have searched through all the <a href="https://www.integromat.com/cs/integrations" target="_blank">apps/services</a> that Integromat already supports and did not find the one you would like to employ in your scenario, you are in the right place. This DIY guide will help you to create an Integromat App for the app/service yourself without writing a piece of code. The only requirement is that the app/service provides an API. Then all you need to do is put together a few declarations in a simple JSON format.

In case you would like to try to create your new Integromat App without any particular app/service in mind, we provide a Virtual Library Demo API that you can freely experiment with and that we use throughout this guide in various examples.

**TIP** To see if there is an API provided by the app/service you would like to integrate, google this: `API site:www.app-or-service.com`, for example: <a href="https://www.google.cz/search?q=API+site%3Awww.eventbrite.com" target="_blank">`API site:www.eventbrite.com`</a>

In the left main menu choose `My Apps`. The (probably still empty) list of all your Apps will be shown. Click the button `Create a new App` in the right top corner. A dialog will pop up, where you can set some basic properties of your new App like its name etc. For the moment leave default values and click `Save`.

![Create a new App](images/create-an-app.png)

Your new App will appear in the list. Click on your new App. A page with five tabs will be shown: `Base`, `Connections`, `Webhooks`, `Modules` and `Remote Procedures` these are the main components your App will be composed of.

## Basic settings

The `Base` tab contains basic setting used by the other components of the module. You can see a dummy JSON snippet:

```json
{
    "baseUrl": "https://www.example.com"
}
```

Replace the url address https://www.example.com with the API url. In case you would like to use our Demo API, use http://demo-api.integrokit.com/api/

```json
{
    "baseUrl": "http://demo-api.integrokit.com/api/"
}
```

## Creating a module

Modules are the key component of your App. They are basically wrappers around specific app/service functionality, which is exposed via an API endpoint. There are three basic types of modules: `Actions`, `Searches` and `Triggers`. 

| Type | Description |
| --- | --- |
| **[Actions](../action.html)** | Use if the API endpoint returns a single response. Examples are Insert a book, Remove a book or Get book info. |
| **[Searches](../searche.html)** | Use if the API endpoint returns multiple items. An example is List books that will find specific books according to search criteria. |
| **[Triggers](../trigger.html)** | Use if you wish to watch for any changes in your app/service. Examples are Watch new book, which will be triggered whenever a new book has been added to the library |

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
user.

Click the tab `Connections`. The (probably still empty) list of all your Connections will be shown. Click the large button with plus sign in the right top corner and choose `Create a new Connection` from the dropdown menu. A dialog will pop up, where you can name your Connection and choose its type. Fill the dialog as shown and click `Save`.

![Create a new Connection](images/create-a-connection.png)

The new Connection will appear in the list. Click the new Connection. A page with two tabs will be shown: `Communication` and `Parameters`.

A pre-configured communication will look like this:

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

