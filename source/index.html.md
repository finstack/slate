---
title: Finstack API Reference

language_tabs:
  - http: HTTP

toc_footers:
  - <a href='https://app.finstack.io/bo/settings'>Sign Up for an API Key</a>
  - <a href='http://swagger.io/'>API Specification Powered by Swagger</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Finstack API! You can use our API to access Finstack API endpoints, which can get information on various users, bank accounts and mandates.

The Finstack API follows [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer) principles in a pragmatic (non-dogmatic) way.
Focus is on simplicity, consistency and security; less on discoverability.

Except for rare cases such as PDF document upload and download, Finstack API exclusively consumes and produces `application/json` content.

The base URL of the production API is __https://app.finstack.io/api/v1__. For testing purposes, you can use __https://sandbox.finstack.io/api/v1__

The current version is __1.0.1__.

You can contact us at __contact@finstack.io__.

[Terms of service](https://cdn2.hubspot.net/hubfs/2126316/Legal%20Documents/Finstack%20Terms%20&%20Conditions.pdf)

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-Auth-Token: myapikeyvalue"
```

> Make sure to replace `myapikeyvalue` with your API key.

Finstack uses API keys to allow access to the API. You can register a new Finstack API key at our [back office](https://app.finstack.io/bo/settings).

Finstack expects for the API key to be included in all API requests to the server in a header that looks like the following:

`X-Auth-Token: myapikeyvalue`

There are three types of API keys you can create:

 - Read only keys: Used for reading data.
 - Read and write keys: They offer access to all methods of the API. They should be kept in a safe place and renewed frequently.
 - Public keys: They can be shared publicly and are only used to initiate mandate signature on an e-commerce website.

<aside class="notice">Carefully pick the API key you need for your use case.</aside>

<aside class="warning">If you're using the wrong type of key, some requests will return 403 Forbidden.</aside>

<aside class="success">Remember — a happy user is an authenticated user!</aside>

# Users

## Find Users

```http
GET /api/v1/users HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "name": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Get User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Update User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
                    "verb": "GET"
                }
            ]            
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

The Users endpoint returns information about *Finstack* users you created,
whether they are creditors or simply end users. The response includes 
the display name and other details about each user.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Optional. Offset the list of returned results by this amount.
limit | query | integer | 20 | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [ShortUserArray](#shortuserarray) | An array of short users.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

## Create User

```http
POST /api/v1/users HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T18:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "UserAlreadyExists",
    "message": "User with email foo@example.com already exists",
    "fields": {
        "emailAddress": "foo@example.com"
    }
}
```

Creates a managed user. Be careful, a user is identified uniquely be her email. 
Therefore, when you call this method and provide an email already known you will get an error.
Unless the optional `isUpdateAllowed` parameter is set to true, in which case it will 
update the existing user with the new details.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
isUpdateAllowed | query | boolean | false | Optional. This parameter allows to update an existing user than throw an error if she already exists. Only use it when your system cannot garanty the unicity of user accounts and their associated emails.
user<b title="required">&nbsp;*&nbsp;</b> | body | [NewUser](#newuser) | |

### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | The updated user, if `isUpdateAllowed` is set to `true`, and the email provided belongs to an existing user.<br/>
201 | [User](#user) | The newly created user.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Find Users with Details

```http
GET /api/v1/users/full HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Update User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
                    "verb": "GET"
                }
            ],
            "email": "user@example.com",
            "firstName": "Marc",
            "lastName": "Dupont",
            "mobile": "+33612345678",
            "address": {
                "street": "82, avenue du général Leclerc",
                "postCode": "75014",
                "city": "PARIS",
                "country": "France"
            },
            "sci": "DE98ZZZ09999999999",
            "legalEntity": {
                "category": "Company",
                "name": "Acme",
                "vatNumber": "YOLO"
            },
            "metadata": "custom data"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

The Users endpoint returns information about *Finstack* users you created,
whether they are creditors or simply end users. The response includes 
the display name and other details about each user.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Optional. Offset the list of returned results by this amount.
limit | query | integer | 20 | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [UserArray](#userarray) | An array of users with details.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get All User Events

```http
GET /api/v1/users/events?since=2015-01-01T12:00:00.000Z HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "events": [
        {
            "resourceType": "User",
            "resourceLink": {
                "rel": "Get User",
                "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "UserCreated"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns all events on users since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [EventArray](#eventarray) | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get User

```http
GET /api/v1/users/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/users/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/users/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns the user with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | User found.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a user you don't manage.
404 | [Error](#error) | User not found.

## Update User

```http
PUT /api/v1/users/{id} HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T18:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/users/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/users/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Updates the user profile.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.
user<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [ModifiedUser](#modifieduser) | 

### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | The updated user.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to modify a user you don't manage.
404 | [Error](#error) | User not found.

## Delete User

```http
DELETE /api/v1/users/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/users/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/users/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Deletes all information (mandates, bank accounts, events and user account) relative to the user with the specified id. WARNING: This action is only available in a SANDBOX environment. Use this only for automated testing.



### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | Deactivated user.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to delete a user you don't manage.
404 | [Error](#error) | User not found.


## Find Bank Accounts of User

```http
GET /api/v1/users/{id}/bank_accounts HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "status": "Verified",
        "createdAt": "2015-01-01T12:00:00.000Z",
        "updatedAt": "2015-01-01T12:00:00.000Z",
        "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
        "links": [
            {
                "rel": "Disable Bank Account",
                "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            },
            {
                "rel": "Get User",
                "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
                "verb": "GET"
            }
        ],
        "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
        "holder": "Marc Dupont",
        "bic": "SOGEFRPPXXX",
        "iban": "FR**********************606",
        "metadata": "custom data"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/users/toto/bank_accccount' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/users/toto/bank_accccount"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all bank accounts that belong to the specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[BankAccount](#bankaccount)] | An array of bank accounts.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to access bank accounts of a user that you don't manage.
404 | [Error](#error) | User not found.


## Find Mandates of User

```http
GET /api/v1/users/{id}/mandates HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "SignedElectronically",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T18:00:00.000Z",
            "payerName": "Payer SAS",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ]
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all mandates that belong to the specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.
role | query | string | Optional. Filter mandates depending on role. Possible values are 'all', 'asPayer', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all mandates.<br/>
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [ShortSDDMandateArray](#shortsddmandatearray) | An array of mandates.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to access mandates of a user that you don't manage.
404 | [Error](#error) | User not found.


## Find Mandates of User with Details

```http
GET /api/v1/users/{id}/mandates/full HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 2,
            "status": "SignedElectronically",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "payerName": "Payer SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "payerEmail": "payer@example.com",
            "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "scheme": "Core",
            "isRecurring": true,
            "umr": "ASPECIALUMR",
            "metadata": "custom data"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all mandates that belong to the specified user with details.

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.
role | query | string | Optional. Filter mandates depending on role. Possible values are 'all', 'asPayer', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all mandates.<br/>
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDMandateArray](#sddmandatearray) | An array of mandates with details.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to access mandates of a user that you don't manage.
404 | [Error](#error) | User not found.


## Find Payments of User

```http
GET /api/v1/users/{id}/payments HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "20",
    "count": "1",
    "payments": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "2",
            "status": "SubmittedToPSP",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
            "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
            "finalCreditorName": "Alice Jones",
            "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
            "payerName": "Bob Adams",
            "amount": 100.00,
            "currency": "EUR",
            "label": "Party bill",
            "valueDate": "2015-02-02",
            "fee": 1.00,
            "canBeCanceled": "false",
            "metadata": {}
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all payments that belong to a specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.
role | query | string | Optional. Filter payments depending on role. Possible values are 'all', 'asPayer', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all payments.<br/>
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDPaymentArray](#sddpaymentarray) | An array of payments.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to access payments of a user that you don't manage.
404 | [Error](#error) | User not found.


## Get Events for User

```http
GET /api/v1/users/{id}/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "resourceType": "User",
        "resourceLink": {
            "rel": "Get User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "timestamp": "2015-01-01T12:00:00.000Z",
        "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
        "eventType": "UserCreated"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all events for a given user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the user.

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Event](#event)] | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to access events on a user that you don't manage.
404 | [Error](#error) | User not found.


## Find User Id

```http
GET /api/v1/users/id/{email} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

"341d533a-d90f-4fce-9fc0-12072f7bd555"
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidEmailAddress",
    "message": "String 'lolo' is not a valid email address",
    "fields": {
        "email": "lolo"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserWithEmailNotFound",
    "message": "User toto@example.com was not found",
    "fields": {
        "email": "toto@example.com"
    }
}
```

Find the user id by email.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
email<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | Email address of the user.

### Responses
Http code | Type | Description
--- | --- | ---
200 | string | UUID of the user.
400 | [Error](#error) | Bad request, occurs when the string passed is not an email address.
404 | [Error](#error) | User not found.


## Get My User Profile

```http
GET /api/v1/users/me HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/meeee' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/meeee"
    }
}
```

Returns the profile information of the authenticated user.


### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | Profile information.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

# Bank Accounts

## Find Bank Accounts

```http
GET /api/v1/bank_accounts HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-01-01T12:00:00.000Z",
            "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
            "links": [
                {
                    "rel": "Disable Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                },
                {
                    "rel": "Get User",
                    "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
                    "verb": "GET"
                }
            ],
            "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR**********************606",
            "metadata": "custom data"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns bank accounts that belong to users that you manage, either because you created them or because 
they authorized you to access their information.



### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Optional. Offset the list of returned results by this amount.
limit | query | integer | 20 | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [BankAccountArray](#bankaccountarray) | An array of bank accounts.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

## Create Bank Account

```http
POST /api/v1/bank_accounts HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
    "metadata": "custom data"
}
```

> HTTP response example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
    "links": [
        {
            "rel": "Disable Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "verb": "GET"
        }
    ],
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR**********************606",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/bank' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/bank"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "UserForbidden",
    "message": "You cannot access user 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage it",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "UserNotFound",
    "message": "User 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "userId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Creates a managed bank account or returns the existing bank account if already in the system.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
bankAccount<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [NewBankAccount](#newbankaccount) | 

### Responses
Http code | Type | Description
--- | --- | ---
201 | [BankAccount](#bankaccount) | The newly created bank account.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
404 | [Error](#error) | User not found.


## Get All Bank Account Events

```http
GET /api/v1/bank_accounts/events?since=2015-01-01T12:00:00.000Z HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "events": [
        {
            "resourceType": "BankAccount",
            "resourceLink": {
                "rel": "Get Bank Account",
                "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "BankAccountCreated"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns all events on bank accounts since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [EventArray](#eventarray) | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get Bank Account

```http
GET /api/v1/bank_accounts/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
    "links": [
        {
            "rel": "Disable Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "verb": "GET"
        }
    ],
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR**********************606",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/bank/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/bank/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "BankAccountForbidden",
    "message": "You cannot view bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 because you do not manage its owner",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "BankAccountNotFound",
    "message": "Bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 was not found",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```


Gets the bank account with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the bank account.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [BankAccount](#bankaccount) | Bank account found.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a bank account belonging to a user you don't manage.
404 | [Error](#error) | Bank account not found.

## Disable Bank Account

```http
DELETE /api/v1/bank_accounts/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "status": "Disabled",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T18:00:00.000Z",
    "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
    "links": [],
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR**********************606",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/bank' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/bank"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "BankAccountForbidden",
    "message": "You cannot view bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 because you do not manage its owner",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "BankAccountNotFound",
    "message": "Bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 was not found",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```

Disables the bank account with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the bank account.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [BankAccount](#bankaccount) | Disabled bank account.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you disable a bank account belonging to a user you don't manage.
404 | [Error](#error) | Bank account not found.


## Get Events for Bank Account

```http
GET /api/v1/bank_accounts/{id}/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "resourceType": "BankAccount",
        "resourceLink": {
            "rel": "Get Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "timestamp": "2015-01-01T12:00:00.000Z",
        "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
        "eventType": "BankAccountCreated"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/bank' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/bank"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "BankAccountForbidden",
    "message": "You cannot view bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 because you do not manage its owner",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "BankAccountNotFound",
    "message": "Bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 was not found",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```

Returns all events for a given bank account.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the bank account.

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Event](#event)] | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to request bank account events for a user that you don't manage.
404 | [Error](#error) | Bank account not found.


## Find My Bank Accounts

```http
GET /api/v1/bank_accounts/me HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "status": "Verified",
        "createdAt": "2015-01-01T12:00:00.000Z",
        "updatedAt": "2015-01-01T12:00:00.000Z",
        "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
        "links": [
            {
                "rel": "Disable Bank Account",
                "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            },
            {
                "rel": "Get User",
                "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
                "verb": "GET"
            }
        ],
        "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
        "holder": "Marc Dupont",
        "bic": "SOGEFRPPXXX",
        "iban": "FR**********************606",
        "metadata": "custom data"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/bank/me' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/bank/me"
    }
}
```

Finds all bank accounts that belong to the authenticated user.


### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[BankAccount](#bankaccount)] | An array of bank accounts.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

# Mandates

## Find Mandates

```http
GET /api/v1/mandates HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "SignedElectronically",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "payerName": "Payer SAS",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ]
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns mandates for which you are a counterparty.


### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
role | query | string | all | Optional. Filter mandates depending on role. Possible values are &#039;all&#039;, &#039;asPayer&#039;, &#039;asCreditor&#039; and &#039;asThirdPartyCreditor&#039;. By default, return all mandates.
offset | query | integer | 0 | Optional. Offset the list of returned results by this amount.
limit | query | integer | 20 | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [ShortSDDMandateArray](#shortsddmandatearray) | An array of mandates.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

## Create Mandate

```http
POST /api/v1/mandates HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "finalCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "sendSignatureRequestByEmail": false,
    "signElectronically": true,
    "metadata": "custom data"
}
```

> HTTP response example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "status": "ToBeSigned",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "payerName": "Payer SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorName": "Finstack",
    "thirdPartyCreditorName": "3rd party Company",
    "links": [
        {
            "rel": "Get Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        {
            "rel": "Sign Mandate",
            "href": "https://app.finstack.io/bo/mandate/341d533a-d90f-4fce-9fc0-12072f7bd555/sign/mkqdfnmjqmfmqjd7689869",
            "verb": "GET"
        },
        {
            "rel": "Get Payer",
            "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Payer Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Cancel Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "payerEmail": "payer@example.com",
    "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "signElectronically": true,
    "fee": 0.99,
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidJSON",
    "message": "...",
    "fields": "..."
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "InvalidKeyForOperation",
    "message": "You are not allowed to execute a Write operation with a Read API key",
    "fields": {
        "apiKeyId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
        "apiKeyRole": "Read"
        "operationType": "Write"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "BankAccountNotFound",
    "message": "Bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 was not found",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```

Creates a payment mandate to be signed.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
mandate<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [NewSDDMandate](#newsddmandate) | 

### Responses
Http code | Type | Description
--- | --- | ---
201 | [SDDMandate](#sddmandate) | The newly created mandate.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to create a mandate with the wrong API key for example.
404 | [Error](#error) | Bank account or user not found.


## Find Mandates with Details

```http
GET /api/v1/mandates/full HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 2,
            "status": "SignedElectronically",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "payerName": "Payer SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "payerEmail": "payer@example.com",
            "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "scheme": "Core",
            "isRecurring": true,
            "umr": "ASPECIALUMR",
            "signElectronically": true,
            "fee": 0.99,
            "metadata": "custom data"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns mandates for which you are a counterparty.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
role | query | string | Optional. Filter mandates depending on role. Possible values are 'all', 'asPayer', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all mandates.<br/>
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDMandateArray](#sddmandatearray) | An array of mandates with details.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get All Mandate Events

```http
GET /api/v1/mandates/events?since=2015-01-01T12:00:00.000Z HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "events": [
        {
            "resourceType": "SDDMandate",
            "resourceLink": {
                "rel": "Get Mandate",
                "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "SDDMandateRequested"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns all events on mandates since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [EventArray](#eventarray) | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get Mandate

```http
GET /api/v1/mandates/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "status": "SignedElectronically",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "payerName": "Payer SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorName": "Creditor SARL",
    "thirdPartyCreditorName": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Payer",
            "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Payer Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "payerEmail": "payer@example.com",
    "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "signElectronically": true,
    "fee": 0.99,
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/mandates/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/mandates/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "MandateForbidden",
    "message": "You cannot access mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Gets the mandate with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the mandate.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDMandate](#sddmandate) | Mandate found.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a mandate where you don't manage any of the parties involved.
404 | [Error](#error) | Mandate not found.

## Cancel or Revoke Mandate

```http
DELETE /api/v1/mandates/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 3,
    "status": "Revoked",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "payerName": "Payer SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorName": "Creditor SARL",
    "thirdPartyCreditorName": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Payer",
            "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Payer Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        }
    ],
    "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "payerEmail": "payer@example.com",
    "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "signElectronically": true,
    "fee": 0.99,
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/mandates/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/mandates/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "MandateForbidden",
    "message": "You cannot access mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Cancels the mandate if it isn&#039;t signed yet or revokes it if it is signed.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the mandate.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDMandate](#sddmandate) | Mandate canceled or revoked.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you cancel a mandate where you don't manage any of the parties involved.
404 | [Error](#error) | Mandate not found.


## Get Signature URL

```http
GET /api/v1/mandates/{id}/sign HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: text/plain

https://app.finstack.io/bo/mandate/a550693f-9743-4e79-bf92-24a023bb81d5/sign/0893af5d-94ef-45e2-89c8-b5c1658d8503
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/mandates/toto/sign' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/mandates/toto/sign"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "MandateForbidden",
    "message": "You cannot access mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns the link to be sent to the payer for her to sign the mandate. Can only be called when the mandate is not signed yet. Use this call only if you lost the signature link sent in the mandate creation request's response.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the mandate.

### Responses
Http code | Type | Description
--- | --- | ---
200 | no content | URL to send to the payer.
400 | [Error](#error) | Bad request, occurs when the mandate is not to be signed (already signed, revoked or canceled).
403 | [Error](#error) | Forbidden, occurs when you request a mandate where you don't manage any of the parties involved.
404 | [Error](#error) | Mandate not found.


## Get Signed Mandate Document

```http
GET /api/v1/mandates/{id}/document HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/mandates/toto/doc' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/mandates/toto/doc"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "MandateForbidden",
    "message": "You cannot access mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns signed mandate document (if any) with the specified id in PDF.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the mandate.

### Responses
Http code | Type | Description
--- | --- | ---
200 | no content | Signed mandate file in PDF format.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a mandate where you don't manage any of the parties involved.
404 | [Error](#error) | Mandate not found.


## Get Events for Mandate

```http
GET /api/v1/mandates/{id}/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "resourceType": "SDDMandate",
        "resourceLink": {
            "rel": "Get Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "timestamp": "2015-01-01T12:00:00.000Z",
        "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
        "eventType": "SDDMandateRequested"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/mandates/toto/events' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/mandates/toto/events"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "MandateForbidden",
    "message": "You cannot access mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all events for a given mandate.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the mandate.

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Event](#event)] | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a mandate where you don't manage any of the parties involved.
404 | [Error](#error) | Mandate not found.


# Payments

## Find Payments

```http
GET /api/v1/payments HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "20",
    "count": "1",
    "payments": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "2",
            "status": "SubmittedToPSP",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
            "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
            "finalCreditorName": "Alice Jones",
            "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
            "payerName": "Bob Adams",
            "amount": 100.00,
            "currency": "EUR",
            "label": "Party bill",
            "valueDate": "2015-02-02",
            "fee": 1.00,
            "canBeCanceled": "false",
            "metadata": {}
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns payments for which you are a counterparty.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
role | query | string | Optional. Filter payments depending on role. Possible values are 'all', 'asPayer', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all payments.<br/>
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDPaymentArray](#sddpaymentarray) | An array of payments.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

## Create Payment

```http
POST /api/v1/payments HTTP/1.1
X-Auth-Token: myapikeyvalue
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "amount": "100.00",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "metadata": {}
}
```

> HTTP response example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "1",
    "status": "PendingMandateValidation",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
    "finalCreditorName": "Alice Jones",
    "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
    "payerName": "Bob Adams",
    "amount": 100.00,
    "currency": "EUR",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "fee": 0.00,
    "canBeCanceled": "true",
    "metadata": {}
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidJSON",
    "message": "...",
    "fields": "..."
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "InvalidKeyForOperation",
    "message": "You are not allowed to execute a Write operation with a Read API key",
    "fields": {
        "apiKeyId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
        "apiKeyRole": "Read"
        "operationType": "Write"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "MandateNotFound",
    "message": "Mandate 02b331d1-f938-4ac4-ab40-ac287c8e8c61 was not found",
    "fields": {
        "mandateId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```

Creates a payment to withdraw money from a payer's bank account.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
payment<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [NewSDDPayment](#newsddpayment) | 

### Responses
Http code | Type | Description
--- | --- | ---
201 | [SDDPayment](#sddpayment) | The newly created payment.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to create a payment with the wrong API key for example.
404 | [Error](#error) | Mandate not found.


## Get All Payment Events

```http
GET /api/v1/payments/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "20",
    "count": "1",
    "events": [
        {
            "resourceType": "SDDPayment",
            "resourceLink": {
                "rel": "Get Payment",
                "href": "https://app.finstack.io/api/v1/payments/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "SDDPaymentInitiated"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns all events on payments since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [EventArray](#eventarray) | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get Payment

```http
GET /api/v1/payments/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "1",
    "status": "PendingMandateValidation",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
    "finalCreditorName": "Alice Jones",
    "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
    "payerName": "Bob Adams",
    "amount": 100.00,
    "currency": "EUR",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "fee": 0.00,
    "canBeCanceled": "true",
    "metadata": {}
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/payments/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/payments/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "PaymentForbidden",
    "message": "You cannot view payment 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "paymentId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "PaymentNotFound",
    "message": "Direct debit 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Gets the payment with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the payment.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDPayment](#sddpayment) | Direct debit found.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a payment where you don't manage any of the parties involved.
404 | [Error](#error) | Direct debit not found.


## Get Events for Payment

```http
GET /api/v1/payments/{id}/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "resourceType": "SDDPayment",
        "resourceLink": {
            "rel": "Get Payment",
            "href": "https://app.finstack.io/api/v1/payments/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": 1,
        "timestamp": "2015-01-01T12:00:00.000Z",
        "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
        "eventType": "SDDPaymentInitiated"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/payments/toto/events' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/payments/toto/events"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "PaymentForbidden",
    "message": "You cannot view payment 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "paymentId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "PaymentNotFound",
    "message": "Direct debit 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Returns all events for a given payment.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the payment.

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Event](#event)] | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a payment where you don't manage any of the parties involved.
404 | [Error](#error) | Direct debit not found.


## Cancel Payment

```http
POST /api/v1/payments/{id}/cancel HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "2",
    "status": "Canceled",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
    "finalCreditorName": "Alice Jones",
    "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
    "payerName": "Bob Adams",
    "amount": 100.00,
    "currency": "EUR",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "fee": 1.00,
    "canBeCanceled": "false",
    "metadata": {}
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/payments/toto/cancel' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/payments/toto/cancel"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "PaymentForbidden",
    "message": "You cannot view payment 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage one of its parties",
    "fields": {
        "paymentId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "PaymentNotFound",
    "message": "Direct debit 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "mandateId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Cancels the payment while it's still possible.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the payment.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [SDDPayment](#sddpayment) | The canceled payment.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid or when the payment can no longer be canceled.
403 | [Error](#error) | Forbidden, occurs when you cancel a payment where you don't manage any of the parties involved.
404 | [Error](#error) | Direct debit not found.


# Events

## Get All Events

```http
GET /api/v1/events?since=2015-01-01T12:00:00.000Z HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "events": [
        {
            "resourceType": "SDDMandate",
            "resourceLink": {
                "rel": "Get Mandate",
                "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "SDDMandateRequested"
        }
    ]
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "MaximumLimitExceeded",
    "message": "You cannot request 150 elements because it exceeds the maximum limit (100)",
    "fields": {
        "requestedLimit": "150",
         "maximumLimit": "100"
    }
}
```

Returns all events since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
offset | query | integer | Optional. Offset the list of returned results by this amount.
limit | query | integer | Optional. Number of items to retrieve. Default is 20, maximum is 100.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [EventArray](#eventarray) | A list of events ordered chronologically.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get Event Summary

```http
GET /api/v1/events/summary?since=2015-01-01T12:00:00.000Z HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: text/csv

resourceType,eventType,numberOfEvents
BankAccount,BankAccountCreated,4
SDDMandate,SDDMandateRequested,4
SDDMandate,SDDMandateSignedElectronically,1
User,UserCreated,5

```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/event/summary' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/event/summary"
    }
}
```

Returns a CSV summary of events that occurred during the provided time range.
The CSV has 3 columns: resource type, event type and number of events.
The first line is the header of the file and contains column names.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | Return all events after given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
until | query | string | Optional. Return all events before given timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.

### Responses
Http code | Type | Description
--- | --- | ---
200 | string | A list of event counters formatted in CSV.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Event Types

|Resource|Event Type|Description|Origin|
|----|----|----|----|
|BankAccount|BankAccountCreated|Occurs when a bank account is created.|Finstack website or API|
|BankAccount|BankAccountDisabled|Occurs when a bank account is disabled.|API|
|BankAccount|BankAccountUpdatedByPSP|Occurs when the bank account is submitted and analyzed by a Payment Service Provider (PSP).|PSP|
|SDDMandate|PayerRegistered|Occurs after payer registration in Finstack when the mandate is requested manually from the back office and the payer is not yet a Finstack user.|Finstack website|
|SDDMandate|SDDMandateCanceled|Occurs when the creditor cancels a mandate request either from the API or the back office.|Finstack back office or API|
|SDDMandate|SDDMandateRequested|Occurs when a mandate is created (also called requested because a signature request is issued to the payer.|Finstack back office or API|
|SDDMandate|SDDMandateRevoked|Occurs when any of the parties (payer, creditor or third party creditor) revokes the mandate.|Finstack back office or API|
|SDDMandate|SDDMandateSignedElectronically|Occurs when the mandate is electronically signed by the payer.|Finstack website|
|SDDMandate|SDDMandateSignedManually|Occurs when the mandate is manually signed by the payer.|Finstack back office|
|SDDMandate|SDDMandateUsedByPSP|Occurs when the mandate is used for the first time for a payment.|PSP|
|SDDMandate|SignedDocumentStored|Occurs immediately after signature when the signed PDF document is archived.|Finstack system|
|SDDPayment|DirectDebitSubmittedToPSP|Occurs when the payment is submitted to be executed.|Finstack system|
|SDDPayment|SDDPaymentCanceled|Occurs when the payment is canceled by the final creditor either from the back office or the API.|Finstack back office or API|
|SDDPayment|SDDPaymentExecuted|Occurs when the payment is executed.|PSP|
|SDDPayment|SDDPaymentFullySettled|Occurs 8 weeks after the payment is partially settled if there was a rolling reserve, this event indicates that rolling reserve associated with this payment was released.|Finstack system|
|SDDPayment|SDDPaymentInitiated|Occurs when a payment is created.|Finstack website, back office or API|
|SDDPayment|SDDPaymentPartiallySettled|After successful execution, occurs when the amount collected is sent to creditor's bank account before rolling reserve.|Finstack system|
|SDDPayment|SDDPaymentPlanned|Occurs when the mandate associated with the payment is signed.|Finstack website or API|
|SDDPayment|SDDPaymentRejected|Occurs when the payment is charged back.|Payer's bank account|
|User|UserCreated|Occurs when a user is created through the API or when she registers directly at Finstack's website.|Finstack website or API|
|User|UserModified|Occurs when a user is modified.|Finstack website or API|
   

#Webhooks

## Get Webhooks

```http
GET /api/v1/webhooks HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "createdAt": "2015-01-01T12:00:00.000Z",
        "updatedAt": "2015-01-01T12:00:00.000Z",
        "links": [
            {
                "rel": "Update Webhook",
                "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "PUT"
            },
            {
                "rel": "Delete Webhook",
                "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            }
        ],
        "callbackURL": "https://www.yourwebsite.com/webhooklistener",
        "resourcesAndEvents": {
          "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
          "User": [],
          "Wallet": ["PendingFeeAdded"]
        },
        "description": "Listen to specific events on bank accounts and wallets and all events on users."
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/webhook' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/webhook"
    }
}
```

Returns all webhooks.


### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Webhook](#webhook)] | A list of webhooks.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.

## Create Webhook

```http
POST /api/v1/webhooks HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```

> HTTP response example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidJSON",
    "message": "...",
    "fields": "..."
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "InvalidKeyForOperation",
    "message": "You are not allowed to execute a Write operation with a Read API key",
    "fields": {
        "apiKeyId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
        "apiKeyRole": "Read"
        "operationType": "Write"
    }
}
```

Creates a new web hook. All you need to provide is an HTTPS URL that will be used to send you events. 
All other parameters are optional, by default all events are sent. Resources and events parameters act like filters.



### Parameters
Name | In | Type | Description
--- | --- | --- | ---
webhook<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [NewWebhook](#newwebhook) | 

### Responses
Http code | Type | Description
--- | --- | ---
201 | [Webhook](#webhook) | The newly created webhook.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you try to create a payment with the wrong API key for example.


## Get Recent Webhook Failures

```http
GET /api/v1/webhooks/failures HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "occurredAt": "2015-01-01T12:00:00.000Z",
        "error": "TimeoutException"
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/webhooks/failure' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/webhooks/failure"
    }
}
```

Returns the list of failures (if any) on all webhooks in the previous month.
Failures are grouped by webhook ID and ordered chronologically.



### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[WebhookFailure](#webhookfailure)] | A list of failures.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.


## Get Webhook

```http
GET /api/v1/webhooks/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/webhooks/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/webhooks/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "WebhookForbidden",
    "message": "You cannot access webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage its owner",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "WebhookNotFound",
    "message": "Webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Gets the webhook with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the webhook.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Webhook](#webhook) | Webhook found.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you request a webhook belonging to a user you don't manage.
404 | [Error](#error) | Webhook not found.

## Update Webhook

```http
PUT /api/v1/webhooks/{id} HTTP/1.1
Content-Type: application/json

{
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": [],
        "User": []
    },
    "description": "Listen to all events on users and bank accounts."
}
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T18:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": [],
        "User": []
    },
    "description": "Listen to all events on users and bank accounts."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidJSON",
    "message": "...",
    "fields": "..."
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "WebhookForbidden",
    "message": "You cannot access webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage its owner",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "WebhookNotFound",
    "message": "Webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Updates the webhook.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the webhook.
webhook<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [NewWebhook](#newwebhook) | 

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Webhook](#webhook) | The updated webhook.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you update a webhook belonging to a user you don't manage.
404 | [Error](#error) | Webhook not found.

## Delete Webhook

```http
DELETE /api/v1/webhooks/{id} HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "InvalidURL",
    "message": "String 'https://api.finstack.io/api/v1/webhooks/toto' is not a valid URL or service is down",
    "fields": {
        "url": "https://api.finstack.io/api/v1/webhooks/toto"
    }
}
```
```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "code": "WebhookForbidden",
    "message": "You cannot access webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 because you do not manage its owner",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```
```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "code": "WebhookNotFound",
    "message": "Webhook 341d533a-d90f-4fce-9fc0-12072f7bd555 was not found",
    "fields": {
        "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555"
    }
}
```

Deletes a webhook.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | UUID of the webhook.

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Webhook](#webhook) | Deleted webhook.
400 | [Error](#error) | Bad request, occurs most often when parameters passed are invalid.
403 | [Error](#error) | Forbidden, occurs when you delete a webhook belonging to a user you don't manage.
404 | [Error](#error) | Webhook not found.



# Models
## Address
```json
{
    "street": "82, avenue du général Leclerc",
    "postCode": "75014",
    "city": "PARIS",
    "country": "France"
}
```

A physical address object.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
street<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full street information including number, for example '82, avenue du général Leclerc'.
postCode<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | For example '93500'.
city<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | For example 'Paris'.
country<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | For example 'France'.

	
## BankAccount
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
    "links": [
        {
            "rel": "Disable Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "verb": "GET"
        }
    ],
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR**********************606",
    "metadata": "custom data"
}
```

A managed bank account.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
versionNo<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Version number of the object, useful to track changes through events.
status<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>Created</li><li>Verified</li><li>Disabled</li><li>RejectedByBank</li><li>RejectedByPSP</li></ul> | Status of the bank account.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
proofId | string | uuid | ID of the uploaded document (if any) that proves that the bank account belongs to the user.
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Available actions on the bank account.
userId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | The user's id to whom the bank account belongs.
holder<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Holder of the bank account.
bic<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Bank Identifier Code.
iban<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | International Bank Account Number.
metadata | string |  | Custom information goes here.

	
## BankAccountArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-01-01T12:00:00.000Z",
            "proofId": "01e55840-149c-48dc-aadc-0448e066a9f5",
            "links": [
                {
                    "rel": "Disable Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                },
                {
                    "rel": "Get User",
                    "href": "https://app.finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
                    "verb": "GET"
                }
            ],
            "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR**********************606",
            "metadata": "custom data"
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of bank accounts available.
bankAccounts<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[BankAccount](#bankaccount)] |  | 


## Event
```json
{
    "resourceType": "User",
    "resourceLink": {
        "rel": "Get User",
        "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
        "verb": "GET"
    },
    "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "timestamp": "2015-01-01T12:00:00.000Z",
    "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "eventType": "UserCreated"
}
```

An object that describes an event that occurred to a resource. An event is uniquely identified by the couple resourceId - versionNo.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
resourceType<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>BankAccount</li><li>Document</li><li>SDDMandate</li><li>SDDPayment</li><li>User</li></ul> | Type of resource that the event applies to.
resourceLink<span title="required" class="required">&nbsp;*&nbsp;</span> | [Link](#link) |  | Link to follow to get the latest representation of the resource.
resourceId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | ID of the resource affected by the event.
versionNo<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Version number of the resource after the event occurs.
timestamp<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Event timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
commandId<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | ID of the command that triggered the event.
eventType<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Type of the event.


## EventArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "events": [
        {
            "resourceType": "User",
            "resourceLink": {
                "rel": "Get User",
                "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "GET"
            },
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "timestamp": "2015-01-01T12:00:00.000Z",
            "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "eventType": "UserCreated"
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of events available.
events<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Event](#event)] |  | 


## LegalEntity
```json
{
    "category": "Company",
    "name": "Acme",
    "vatNumber": "YOLO"
}
```

Information about all types of corporation such as companies, associations...

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
category | string | <div>Acceptable values:</div><ul class="enum"><li>Company</li><li>Association</li><li>AutoEntrepreneur</li></ul> | Type of legal entity.
name<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
vatNumber | string |  | VAT number (if any).


## Link
```json
{
    "rel": "Get User",
    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
    "verb": "GET"
}
```

An available action on a resource.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
rel<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Describes the action that can be conducted.
href<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uri | URL to execute the action.
verb<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>GET</li><li>POST</li><li>PUT</li><li>DELETE</li></ul> | HTTP verb required to execute the action.


## ModifiedUser
```json
{
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "en",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
email | string | email | 
firstName | string |  | 
lastName | string |  | 
mobile | string |  | Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.
address | [Address](#address) |  | A physical address object.
preferredLang | string |  | ISO code of the preferred language of the user, 'fr' for example.
sci | string |  | SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.
legalEntity | [LegalEntity](#legalentity) |  | Provide this field if the user is not an individual.
metadata | string |  | Custom information goes here.

	
## NewBankAccount
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
    "metadata": "custom data"
}
```

Bank account information required to declare a new bank account.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string. You are required to generate the id of the bank account in order to make the call idempotent. That allows you to safely retry to create the bank account without fearing to create multiple accounts on our side in case there was a network problem.
userId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | The user's id to whom the bank account belongs.
holder | string |  | Holder of the bank account. By default, it is equal to the full name of the user or it's legal entity name.
bic<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Bank Identifier Code.
iban<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | International Bank Account Number.
metadata | string |  | Custom information goes here.


## NewSDDMandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "payerEmail": "payer@example.com",
    "finalCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "sendSignatureRequestByEmail": false,
    "signElectronically": true,
    "metadata": "custom data"
}
```

Information required to issue a new mandate.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string. You are required to generate the id of the mandate in order to make the call idempotent. That allows you to safely retry to create the mandate without fearing to create multiple mandates on our side in case there was a network problem.
payerEmail | string | email | The payer's email. If you already know the payer's bank account id, do not use this parameter.
payerBankAccountId | string |  | The payer's bank account id. If you do not know it, you can specify the payer's email instead.
finalCreditorId | string |  | Use this field only if the final creditor of the mandate is not the API user.
sendSignatureRequestByEmail | boolean |  | Set this flag to 'true' if you want to send a mandate signature request by email through Finstack.
signElectronically | boolean |  | Set this flag to 'true' if you want the mandate to be signed electronically by the payer. 'true' if not specified.
redirectURL | string |  | Redirect payer to this URL after mandate is signed or accepted.
umr | string |  | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters. If not specified, we generate one for you.
metadata | string |  | Custom information goes here.


## NewSDDPayment
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "amount": "100.00",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "metadata": {}
}
```

Information required to issue a new SEPA Direct Debit (SDD) payment.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string. You are required to generate the id of the payment in order to make the call idempotent. That allows you to safely retry to create the payment without fearing to create multiple debits on our side in case there was a network problem.
mandateId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | ID of the associated mandate. The mandate will be used to know who is the creditor and who is the payer in this transaction.
amount<span title="required" class="required">&nbsp;*&nbsp;</span> | number |  | Amount to be withdrawn from the payer's bank account, for example '100.00'. Minimum is 1€ and maximum is 20000€, any amounts outside this range will be rejected.
label<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Short message that will appear on the payer's bank account statement.
valueDate | string | date | Date when the payment is charged, for example '2015-01-01'. If not specified or date is in the past, the payment will be executed as soon as possible.
metadata | string |  | Custom information goes here.


## NewUser
```json
{
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
email<span title="required" class="required">&nbsp;*&nbsp;</span> | string | email | 
firstName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
lastName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
mobile<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.
address | [Address](#address) |  | A physical address object.
preferredLang | string |  | ISO code of the preferred language of the user, 'fr' for example.
sci | string |  | SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.
legalEntity | [LegalEntity](#legalentity) |  | Provide this field if the user is not an individual.
metadata | string |  | Custom information goes here.


## NewWebhook
```json
{
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```

Information required to create a new webhook.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
callbackURL<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uri | Finstack will do a POST call to this URL to send events. The URL has to be HTTPS in production!
resourcesAndEvents | object |  | A map where keys are resource types - BankAccount, Document, Plan, SDDMandate, SDDPayment, Subscription, User and Wallet. Values are arrays of the names of the events that can occur to the resource. If the array is empty, the webhook will listen to all events related to the corresponding resource type. This parameter is optional, if it isn't provided the webhook will listen to all events.
description | string |  | Optional description.


## SDDMandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "status": "SignedElectronically",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "payerName": "Payer SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorName": "Creditor SARL",
    "thirdPartyCreditorName": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Payer",
            "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Payer Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "payerEmail": "payer@example.com",
    "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "payerMobile": "+33650000000",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "signElectronically": true,
    "fee": 0.99,
    "metadata": "custom data"
}
```

A managed mandate.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
versionNo<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Version number of the object, useful to track changes through events.
status<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>Canceled</li><li>PendingPayerRegistration</li><li>ToBeSigned</li><li>AcceptedOnline</li><li>SignedElectronically</li><li>SignedManually</li><li>SignedAndReceivedByPayersBank</li><li>Revoked</li></ul> | Status of the mandate.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
payerName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name of the payer whether it's an individual or legal entity.<br/>It is taken from the 'holder' field of the bank account!<br/>
payerEmail<span title="required" class="required">&nbsp;*&nbsp;</span> | string | email | The payer's email.
payerMobile | string | | Mobile phone number of the payer (if available) respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.
sci<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.
creditorName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name of the creditor whether it's an individual or legal entity.
thirdPartyCreditorName | string |  | Full name of the third party creditor (if any) whether it's an individual or legal entity.<br/>
signatureDate | string | date | Signature date, for example '2015-01-01'.
documentId | string | uuid | ID of the signed PDF document (if any).
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Available actions on the mandate.
payerId | string | uuid | The payer's user id.
payerBankAccountId | string | uuid | The payer's bank account id.
creditorId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | The creditor's user id. The creditor must be a corporation that owns an SCI.
thirdPartyCreditorId | string |  | The third party creditor's (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.<br/>
scheme<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>Core</li><li>B2B</li></ul> | Can be 'Core' or 'B2B'.
isRecurring<span title="required" class="required">&nbsp;*&nbsp;</span> | boolean |  | Is 'true' by default.
umr<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.
signElectronically<span title="required" class="required">&nbsp;*&nbsp;</span> | boolean |  | Is 'true' if the mandate requires electronic signature.
redirectURL | string |  | Redirect payer to this URL after mandate is signed or accepted.
fee<span title="required" class="required">&nbsp;*&nbsp;</span> | number |  | Fee charged to the final creditor for the mandate only if signed electronically in EUR, '0.99' for example.
metadata | string |  | Custom information goes here.


## SDDMandateArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 2,
            "status": "SignedManually",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "payerName": "Payer SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "payerId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "payerBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "payerEmail": "payer@example.com",
            "payerMobile": "+33650000000",
            "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "scheme": "Core",
            "isRecurring": true,
            "umr": "ASPECIALUMR",
            "signElectronically": true,
            "fee": 0.99,
            "metadata": "custom data"
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of mandates available.
mandates<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[SDDMandate](#sddmandate)] |  | 


## SDDPayment
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "1",
    "status": "PendingMandateValidation",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
    "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
    "finalCreditorName": "Alice Jones",
    "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
    "payerName": "Bob Adams",
    "amount": 100.00,
    "currency": "EUR",
    "label": "Party bill",
    "valueDate": "2015-02-02",
    "fee": 0.00,
    "canBeCanceled": "true",
    "metadata": {}
}
```

Desribes a payment via SEPA Direct Debit (SDD) payment.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
versionNo<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Version number of the object, useful to track changes through events.
status<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>PendingMandateValidation</li><li>Planned</li><li>Canceled</li><li>SubmittedToPSP</li><li>Executed</li><li>PartiallySettled</li><li>FullySettled</li><li>ChargedBack</li></ul> | Status of the payment.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
mandateId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | ID of the associated mandate.
finalCreditorId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | ID of the final creditor. The final creditor is either the third party creditor if there is one or else the creditor.
finalCreditorName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name or legal entity name of the final creditor.
payerId | string | uuid | ID of the payer.
payerName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name or legal entity name of the final creditor.
amount<span title="required" class="required">&nbsp;*&nbsp;</span> | number |  | Amount to be withdrawn from the payer's bank account, for example '100.00'. Minimum is 1€ and maximum is 20000€, any amounts outside this range will be rejected.
currency<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Always equal to 'EUR'.
label<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Short message that will appear on the payer's bank account statement.
valueDate<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date | Date when the payment is charged, for example '2015-01-01'.
fee<span title="required" class="required">&nbsp;*&nbsp;</span> | number |  | Fee charged to the creditor for the payment once executed or chargedback in EUR, '0.50' for example.
canBeCanceled<span title="required" class="required">&nbsp;*&nbsp;</span> | boolean |  | Is 'true' if the payment can still be canceled, 'false' otherwise.
metadata | string |  | Custom information goes here.

	
## SDDPaymentArray
```json
{
    "offset": "0",
    "limit": "20",
    "count": "1",
    "payments": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "status": "PendingMandateValidation",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "mandateId": "3b10dc0b-f123-4d9c-8504-72dc76aa716c",
            "finalCreditorId": "75068e62-1c01-42dd-a125-f79910124615",
            "finalCreditorName": "Alice Jones",
            "payerId": "9c0498f0-df15-40f4-89b9-5af16f846c31",
            "payerName": "Bob Adams",
            "amount": 100.00,
            "currency": "EUR",
            "label": "Party bill",
            "valueDate": "2015-02-02",
            "fee": 1.00,
            "canBeCanceled": "true",
            "metadata": {}
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of payments available.
payments<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[SDDPayment](#sddpayment)] |  | 


## ShortSDDMandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "umr": "ASPECIALUMR",
    "status": "SignedManually",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T18:00:00.000Z",
    "payerName": "Payer SAS",
    "creditorName": "Creditor SARL",
    "thirdPartyCreditorName": "3rd party Company",
    "signatureDate": "2015-02-01",
    "links": [
        {
            "rel": "Get Mandate Details",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        {
            "rel": "Get Payer",
            "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Payer Bank Account",
            "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
           "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ]
}
```

Minimal information about a mandate.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
umr<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.
status<span title="required" class="required">&nbsp;*&nbsp;</span> | string | <div>Acceptable values:</div><ul class="enum"><li>Canceled</li><li>PendingPayerRegistration</li><li>ToBeSigned</li><li>AcceptedOnline</li><li>SignedElectronically</li><li>SignedManually</li><li>SignedAndReceivedByPayersBank</li><li>Revoked</li></ul> | Status of the mandate.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
payerName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name of the payer whether it's an individual or legal entity. It is taken from the 'holder' field of the bank account!
creditorName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Full name of the creditor whether it's an individual or legal entity.
thirdPartyCreditorName | string |  | Full name of the third party creditor (if any) whether it's an individual or legal entity.
signatureDate | string | date | Signature date, for example '2015-01-01'.
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Available actions on the mandate.


## ShortSDDMandateArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "SignedManually",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T18:00:00.000Z",
            "payerName": "Payer SAS",
            "creditorName": "Creditor SARL",
            "thirdPartyCreditorName": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer",
                    "href": "https://app.finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Payer Bank Account",
                    "href": "https://app.finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://app.finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://app.finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ]
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of mandates available.
mandates<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[ShortSDDMandate](#shortsddmandate)] |  | 


## ShortUser
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "name": "An Awesome Company",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Get User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ]
}
```

Minimal information about a user.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
name<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | If the user is an individual, this field displays the first name and the last name, otherwise it would contain the name of the legal entity.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Actions available on the user.


## ShortUserArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "name": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Get User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Update User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
                    "verb": "GET"
                }
            ]
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of users available.
users<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[ShortUser](#shortuser)] |  | 


## User
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 1,
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ],
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "+33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "preferredLang": "fr",
    "sci": "DE98ZZZ09999999999",
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "vatNumber": "YOLO"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
versionNo<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Version number of the object, useful to track changes through events.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Available actions on the user.
email<span title="required" class="required">&nbsp;*&nbsp;</span> | string | email | 
firstName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
lastName<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
mobile<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.
address | [Address](#address) |  | A physical address object.
preferredLang<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | ISO code of the preferred language of the user, 'fr' for example.
sci | string |  | SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.
legalEntity | [LegalEntity](#legalentity) |  | Provide this field if the user is not an individual.
metadata | string |  | Custom information goes here.


## UserArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 1,
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Update User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://app.finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
                    "verb": "GET"
                }
            ],
            "email": "user@example.com",
            "firstName": "Marc",
            "lastName": "Dupont",
            "mobile": "+33612345678",
            "address": {
                "street": "82, avenue du général Leclerc",
                "postCode": "75014",
                "city": "PARIS",
                "country": "France"
            },
            "sci": "DE98ZZZ09999999999",
            "legalEntity": {
                "category": "Company",
                "name": "Acme",
                "vatNumber": "YOLO"
            },
            "metadata": "custom data"
        }
    ]
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
offset<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Position in pagination.
limit<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Number of items to retrieve (100 max).
count<span title="required" class="required">&nbsp;*&nbsp;</span> | integer | int32 | Total number of users available.
users<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[User](#user)] |  | 


## Webhook
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://app.finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resourcesAndEvents": {
        "BankAccount": ["BankAccountCreated", "BankAccountUpdatedByPSP"],
        "User": [],
        "Wallet": ["PendingFeeAdded"]
    },
    "description": "Listen to specific events on bank accounts and wallets and all events on users."
}
```

A webhook is a subscription to listen to potentially all events in Finstack.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | Should be a valid UUID string.
createdAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
updatedAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.
links<span title="required" class="required">&nbsp;*&nbsp;</span> | array[[Link](#link)] |  | Available actions on the web hook.
callbackURL<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uri | Finstack will do a POST call to this URL to send events. The URL has to be HTTPS!
resourcesAndEvents | object |  | A map where keys are resource types - BankAccount, Document, Plan, SDDMandate, SDDPayment, Subscription, User and Wallet. Values are arrays of the names of the events that can occur to the resource. If the array is empty, the webhook will listen to all events related to the corresponding resource type. If the map is empty, the webhook will listen to all events.
description | string |  | Optional description.


## WebhookFailure
```json
{
    "webhookId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "occurredAt": "2015-01-01T12:00:00.000Z",
    "error": "TimeoutException"
}
```

A webhook failure that occurred when trying to deliver an event.

	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
webhookId<span title="required" class="required">&nbsp;*&nbsp;</span> | string | uuid | The ID of the webhook that failed.
occurredAt<span title="required" class="required">&nbsp;*&nbsp;</span> | string | date-time | Timestamp at which the failure occurred in UTC, for example '2015-01-01T12:00:00.000Z'.
error<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | Error message of the failure

	


<style>
    ul.enum {
        margin: 0;
        padding: 0 0 0 2px;
        list-style-position: inside;
    }
    .required {
        color: red;
        font-weight: bold;
    }
    a.pseudo {
        border-bottom:1px dashed; 
        text-decoration: none;
    }
</style>