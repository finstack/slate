---
title: Finstack API Reference

language_tabs:
  - http: HTTP

toc_footers:
  - <a href='https://finstack.io/admin/settings'>Sign Up for an API Key</a>
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

The base URL of the production API is __https://finstack.io/api/v1__. For testing purposes, you can use __https://sandbox.finstack.io/api/v1__

The current version is __1.0.0__.

You can contact us at __contact@finstack.io__.

[Terms of service](http://finstack.io/terms/)

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-Auth-Token: myapikeyvalue"
```

> Make sure to replace `myapikeyvalue` with your API key.

Finstack uses API keys to allow access to the API. You can register a new Finstack API key at our [admin portal](https://finstack.io/admin/settings).

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
            "designation": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Get User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Update User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

The Users endpoint returns information about *Finstack* users you created,
whether they are merchants or simply end users. The response includes 
the display name and other details about each user.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Offset the list of returned results by this amount.
limit | query | integer | 20 | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortUserArray](#shortuserarray)</td><td>An array of short users.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


## Create User

```http
POST /api/v1/users HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "user": {
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
        "legalEntity": {
            "category": "Company",
            "name": "Acme",
            "sci": "DE98ZZZ09999999999"
        },
        "metadata": "custom data"
    }
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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Creates a managed user. Be careful, a user is identified uniquely be her email. 
Therefore, when you call this method and provide an email already known you will get an error.
Unless the optional `isUpdateAllowed` parameter is set to true, in which case it will 
update the existing user with the new details.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
isUpdateAllowed | query | boolean | false | This parameter allows to update an existing user than throw an error if she already exists. Only use it when your system cannot garanty the unicity of user accounts and their associated emails.
user<b title="required">&nbsp;*&nbsp;</b> | body | [NewUser](#newuser) | |

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>The updated user, if `isUpdateAllowed` is set to `true`, and the email provided belongs to an existing user.
</td></tr> 
<tr><td>201</td><td>[User](#user)</td><td>The newly created user.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
            "legalEntity": {
                "category": "Acme",
                "name": "73282932000074",
                "sci": "DE98ZZZ09999999999"
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

The Users endpoint returns information about *Finstack* users you created,
whether they are merchants or simply end users. The response includes 
the display name and other details about each user.

### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Offset the list of returned results by this amount.
limit | query | integer | 20 | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[UserArray](#userarray)</td><td>An array of users with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
                "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all events on users since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
until | query | string | Return all events before given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 204 No Content
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns the user with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>User found.</td></tr>
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>403</td><td>[Error](#error)</td><td>Forbidden, occurs when you request a user you don't manage.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>User not found.</td></tr>
</table>


## Update User

```http
PUT /api/v1/users/{id} HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "user": {
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
        "legalEntity": {
            "category": "Company",
            "name": "Acme",
            "sci": "DE98ZZZ09999999999"
        },
        "metadata": "custom data"
    }
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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Updates the user profile.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.
user<b title="required">&nbsp;*&nbsp;</b> | body | [ModifiedUser](#modifieduser) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>The updated user.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 204 No Content
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Deletes all information (mandates, bank accounts, events and user account) relative to the user with the specified id. WARNING: This action is only available in a SANDBOX environment. Use this only for automated testing.



### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>Deactivated user.</td></tr> 
<tr><td>204</td><td>no content</td><td>User not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
                "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            },
            {
                "rel": "Get User",
                "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
                "verb": "GET"
           
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all bank accounts that belong to the specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[BankAccount](#bankaccount)]</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T18:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all mandates that belong to the specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.
role | query | string | Filter mandates depending on role. Possible values are &#039;all&#039;, &#039;asDebtor&#039;, &#039;asCreditor&#039; and &#039;asThirdPartyCreditor&#039;. By default, return all mandates.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortMandateArray](#shortmandatearray)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all mandates that belong to the specified user with details.

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.
role | query | string | Filter mandates depending on role. Possible values are &#039;all&#039;, &#039;asDebtor&#039;, &#039;asCreditor&#039; and &#039;asThirdPartyCreditor&#039;. By default, return all mandates.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[MandateArray](#mandatearray)</td><td>An array of mandates with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all events for a given user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[Event](#event)]</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


## Get my User Profile

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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns the profile information of the authenticated user.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>Profile information.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
                    "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                },
                {
                    "rel": "Get User",
                    "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns bank accounts that belong to users that you manage, either because you created them or because 
they authorized you to access their information.



### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Offset the list of returned results by this amount.
limit | query | integer | 20 | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccountArray](#bankaccountarray)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


## Create Bank Account

```http
POST /api/v1/bank_accounts HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "bankAccount": {
        "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
        "holder": "Marc Dupont",
        "bic": "SOGEFRPPXXX",
        "iban": "FR1420041010050500013M02606",
        "metadata": "custom data"
    }
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
            "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Creates a managed bank account.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
bankAccount<b title="required">&nbsp;*&nbsp;</b> | body | [NewBankAccount](#newbankaccount) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>201</td><td>[BankAccount](#bankaccount)</td><td>The newly created bank account.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
                "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all events on bank accounts since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
until | query | string | Return all events before given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
            "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "code": "string",
    "message": "string",
    "fields": "string"
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
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the bank account.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccount](#bankaccount)</td><td>Bank account found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>403</td><td>[Error](#error)</td><td>Forbidden, occurs when you request a bank account belonging to a user you don't manage.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Bank account not found.</td></tr>
</table>

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
    "code": "string",
    "message": "string",
    "fields": "string"
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
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the bank account.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccount](#bankaccount)</td><td>Disabled bank account.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>403</td><td>[Error](#error)</td><td>Forbidden, occurs when you disable a bank account belonging to a user you don't manage.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Bank account not found.</td></tr>
</table>

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
            "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
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
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the bank account.
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[Event](#event)]</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Bank account not found.</td></tr>
</table>

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
                "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            },
            {
                "rel": "Get User",
                "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Finds all bank accounts that belong to the authenticated user.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[BankAccount](#bankaccount)]</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns mandates for which you are a counterparty.


### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
role | query | string | Filter mandates depending on role. Possible values are &#039;all&#039;, &#039;asDebtor&#039;, &#039;asCreditor&#039; and &#039;asThirdPartyCreditor&#039;. By default, return all mandates.
offset | query | integer | 0 | Offset the list of returned results by this amount.
limit | query | integer | 20 | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortMandateArray](#shortmandatearray)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

## Create Mandate

```http
POST /api/v1/mandates HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "mandate": {
        "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
        "finalCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
        "useFinalCreditorSCI": false,
        "metadata": "custom data"
    }
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
    "debtorDesignation": "Debtor SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Lemon Way",
    "thirdPartyCreditorDesignation": "3rd party Company",
    "links": [
        {
            "rel": "Get Debtor",
            "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor Bank Account",
            "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Cancel Mandate",
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Creates a direct debit mandate to be signed.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
mandate<b title="required">&nbsp;*&nbsp;</b> | body | [NewMandate](#newmandate) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>201</td><td>[Mandate](#mandate)</td><td>The newly created mandate.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns mandates for which you are a counterparty.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
role | query | string | Filter mandates depending on role. Possible values are &#039;all&#039;, &#039;asDebtor&#039;, &#039;asCreditor&#039; and &#039;asThirdPartyCreditor&#039;. By default, return all mandates.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[MandateArray](#mandatearray)</td><td>An array of mandates with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
                "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all events on mandates since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
until | query | string | Return all events before given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "debtorDesignation": "Debtor SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Debtor",
            "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor Bank Account",
            "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
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

Gets the mandate with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandate](#mandate)</td><td>Mandate found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
<tr><td>403</td><td>[Error](#error)</td><td>Forbidden, occurs when you request a mandate where you don't manage any of the parties involved.</td></tr> 
<tr><td>404</td><td>[Error](#error)</td><td>Mandate not found.</td></tr> 
</table>

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
    "status": "Disabled",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "debtorDesignation": "Debtor SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Debtor",
            "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor Bank Account",
            "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        }
    ],
    "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "metadata": "custom data"
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
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

Cancels the mandate if it isn&#039;t signed yet or revokes it if it is signed.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandate](#mandate)</td><td>Mandate canceled or revoked.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Mandate not found.</td></tr>
</table>


## Get Signature URL

```http
GET /api/v1/mandates/{id}/sign HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: text/plain

https://finstack.io/admin/mandate/a550693f-9743-4e79-bf92-24a023bb81d5/sign/0893af5d-94ef-45e2-89c8-b5c1658d8503
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
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

Returns the link to be sent to the debtor for her to sign the mandate. Can only be called when the mandate is not signed yet.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>text</td><td>URL to send to the debtor.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs when the mandate is not to be signed (already signed, revoked or canceled).</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Mandate not found.</td></tr>
</table>


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
    "code": "string",
    "message": "string",
    "fields": "string"
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

Returns signed mandate document (if any) with the specified id in PDF.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>no content</td><td>Signed mandate file in PDF format.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Mandate not found.</td></tr>
</table>

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
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
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

Returns all events for a given mandate.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[Event](#event)]</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr>
<tr><td>404</td><td>[Error](#error)</td><td>Mandate not found.</td></tr>
</table>

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
                "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all events since a given date and optionally until another date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
until | query | string | Return all events before given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 20, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
Mandate,SDDMandateRequested,4
Mandate,SDDMandateSigned,1
User,UserCreated,5

```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns a CSV summary of events that occurred during the provided time range.
The CSV has 3 columns: resource type, event type and number of events.
The first line is the header of the file and contains column names.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.
until | query | string | Return all events before given timestamp in UTC, for example &#039;2015-01-01T12:00:00.000Z&#039;.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>string</td><td>A list of event counters formatted in CSV.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


## Event Types

|Resource|Event Type|Description|Origin|
|----|----|----|----|
|BankAccount|BankAccountCreated|Occurs when a bank account is created.|Finstack website or API|
|BankAccount|BankAccountDisabled|Occurs when a bank account is disabled.|API|
|BankAccount|BankAccountUpdatedByPSP|Occurs when the bank account is submitted and analyzed by a Payment Service Provider (PSP).|PSP|
|Mandate|ClientRegistered|Occurs after debtor registration in Finstack when the mandate is requested manually from the admin and the debtor is not yet a Finstack user.|Finstack website|
|Mandate|SDDMandateCanceled|Occurs when the creditor cancels a mandate request either from the API or the admin.|Finstack admin or API|
|Mandate|SDDMandateRejectedByPSP|Occurs when a mandate is submitted to a PSP for user verification (KYC) and this latter fails.|PSP|
|Mandate|SDDMandateRequested|Occurs when a mandate is created (also called requested because a signature request is issued to the debtor.|Finstack admin or API|
|Mandate|SDDMandateRevoked|Occurs when any of the parties (debtor, creditor or third party creditor) revokes the mandate.|Finstack admin or API|
|Mandate|SDDMandateSigned|Occurs when the mandate is electronically signed by the debtor.|Finstack website|
|Mandate|SDDMandateUsedByPSP|Occurs when the mandate is used for the first time for a direct debit.|PSP|
|Mandate|SDDMandateValidatedByPSP|Occurs when the mandate, once signed, is submitted and validated by the PSP.|PSP|
|Mandate|SignedDocumentStored|Occurs immediately after signature when the signed PDF document is archived.|Finstack system|
|User|UserCreated|Occurs when a user is created through the API or when she registers directly at Finstack's website.|Finstack website or API|
   

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
                "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "PUT"
            },
            {
                "rel": "Delete Webhook",
                "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
                "verb": "DELETE"
            }
        ],
        "callbackURL": "https://www.yourwebsite.com/webhooklistener",
        "resources": [
            "User",
            "BankAccount"
        ],
        "events": [
            "SDDMandateRequested"
        ],
        "description": "Listen to all events on users and bank accounts as well as mandate creation events."
    }
]
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Returns all webhooks.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[Webhook](#webhook)]</td><td>A list of webhooks.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

## Create Webhook

```http
POST /api/v1/webhooks HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "webhook": {
         "callbackURL": "https://www.yourwebsite.com/webhooklistener",
         "resources": [
             "User",
             "BankAccount"
         ],
         "events": [
             "SDDMandateRequested"
         ],
         "description": "Listen to all events on users and bank accounts as well as mandate creation events."
    }
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
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [
        "SDDMandateRequested"
    ],
    "description": "Listen to all events on users and bank accounts as well as mandate creation events."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Creates a new web hook. All you need to provide is an HTTPS URL that will be used to send you events. 
All other parameters are optional, by default all events are sent. Resources and events parameters act like filters.
If resources and events are specified then they are both taken into account without sending duplicate events.



### Parameters
Name | In | Type | Description
--- | --- | --- | ---
webhook<b title="required">&nbsp;*&nbsp;</b> | body | [NewWebhook](#newwebhook) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>201</td><td>[Webhook](#webhook)</td><td>The newly created webhook.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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

Returns the list of failures (if any) on all webhooks in the previous month.
Failures are grouped by webhook ID and ordered chronologically.



### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[[WebhookFailure](#webhookfailure)]</td><td>A list of failures.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [
        "SDDMandateRequested"
    ],
    "description": "Listen to all events on users and bank accounts as well as mandate creation events."
}
```
```http
HTTP/1.1 204 No Content
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Gets the webhook with the specified id.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the webhook.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Webhook](#webhook)</td><td>Webhook found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
<tr><td>403</td><td>[Error](#error)</td><td>Forbidden, occurs when you request a webhook belonging to a user you don't manage.</td></tr> 
<tr><td>404</td><td>[Error](#error)</td><td>Webhook not found.</td></tr> 
</table>

## Update webhook

```http
PUT /api/v1/webhooks/{id} HTTP/1.1
Content-Type: application/json

{
    "webhook": {
        "callbackURL": "https://www.yourwebsite.com/webhooklistener",
        "resources": [
            "User",
            "BankAccount"
        ],
        "description": "Listen to all events on users and bank accounts."
    }
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
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [],
    "description": "Listen to all events on users and bank accounts."
}
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Updates the webhook.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the webhook.
webhook<b title="required">&nbsp;*&nbsp;</b> | body | [NewWebhook](#newwebhook) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Webhook](#webhook)</td><td>The updated webhook.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>

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
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [
        "SDDMandateRequested"
    ],
    "description": "Listen to all events on users and bank accounts as well as mandate creation events."
}
```
```http
HTTP/1.1 204 No Content
```
```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```

Deletes a webhook.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the webhook.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Webhook](#webhook)</td><td>Deleted webhook.</td></tr> 
<tr><td>204</td><td>no content</td><td>Webhook not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Bad request, occurs most often when parameters passed are invalid.</td></tr> 
</table>


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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|street|Full street information including number, for example '82, avenue du général Leclerc'.|true|string||
|postCode|For example '93500'.|true|string||
|city|For example 'Paris'.|true|string||
|country|For example 'France'.|true|string|France|

	
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
            "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        },
        {
            "rel": "Get User",
            "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|versionNo|Version number of the object, useful to track changes through events.|true|integer (int32)||
|status|Status of the bank account.|true|enum (ToBeVerified, Verified, Rejected, Disabled)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|proofId|ID of the uploaded document (if any) that proves that the bank account belongs to the user.|false|string||
|links|Available actions on the bank account.|true|array[[Link](#link)]||
|userId|The user's id to whom the bank account belongs.|true|string||
|holder|Holder of the bank account.|true|string||
|bic|Bank Identifier Code.|true|string||
|iban|International Bank Account Number.|true|string||
|metadata|Custom information goes here.|false|string||

	
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
                    "href": "https://finstack.io/api/v1/bank_accounts/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                },
                {
                    "rel": "Get User",
                    "href": "https://finstack.io/api/v1/users/02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of bank accounts available.|true|integer (int32)||
|bankAccounts||true|array[[BankAccount](#bankaccount)]||


## Error
```json
{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|code||true|string||
|message||true|string||
|fields||true|string||


## Event
```json
{
    "resourceType": "User",
    "resourceLink": {
        "rel": "Get User",
        "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|resourceType|Type of resource that the event applies to.|true|enum (BankAccount, Document, SDDMandate, User, Wallet)||
|resourceLink|Link to follow to get the latest representation of the resource.|true|[Link](#link)||
|resourceId|ID of the resource affected by the event.|true|string||
|versionNo|Version number of the resource after the event occurs.|true|integer (int32)||
|timestamp|Event timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|commandId|ID of the command that triggered the event.|true|string||
|eventType|Type of the event.|true|string||


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
                "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of events available.|true|integer (int32)||
|events||true|array[[Event](#event)]||


## LegalEntity
```json
{
    "category": "Company",
    "name": "Acme",
    "sci": "DE98ZZZ09999999999"
}
```

Information about all types of corporation such as companies, associations...

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|category|Type of legal entity.|true|enum (Company, Association, AutoEntrepreneur)|Company|
|name||true|string||
|sci|SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.|false|string||


## Link
```json
{
    "rel": "Get User",
    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
    "verb": "GET"
}
```

An available action on a resource.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|rel|Describes the action that can be conducted.|true|string||
|href|URL to execute the action.|true|string||
|verb|HTTP verb required to execute the action.|true|enum (GET, POST, PUT, DELETE)||

	
## Mandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": 2,
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T12:00:00.000Z",
    "debtorDesignation": "Debtor SAS",
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "links": [
        {
            "rel": "Get Debtor",
            "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor Bank Account",
            "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
            "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditorId": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "scheme": "Core",
    "isRecurring": true,
    "umr": "ASPECIALUMR",
    "metadata": "custom data"
}
```

A managed mandate.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|versionNo|Version number of the object, useful to track changes through events.|true|integer (int32)||
|status|Status of the mandate.|true|enum (Canceled, PendingClientRegistration, ToBeSigned, ToBeValidated, ValidatedNotUsedYet, ValidatedUsed, Disabled, Rejected)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|debtorDesignation|Full name of the debtor whether it's an individual or legal entity. It is taken from the 'holder' field of the bank account!|true|string||
|sci|SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.|true|string||
|creditorDesignation|Full name of the creditor whether it's an individual or legal entity.|true|string||
|thirdPartyCreditorDesignation|Full name of the third party creditor (if any) whether it's an individual or legal entity.|false|string||
|signatureDate|Signature date, for example '2015-01-01'.|false|string (date)||
|documentId|ID of the signed PDF document (if any).|false|string||
|links|Available actions on the mandate.|true|array[[Link](#link)]||
|debtorId|The debtor's user id.|false|string||
|debtorBankAccountId|The debtor's bank account id.|false|string||
|creditorId|The creditor's user id. The creditor must be a corporation that owns an SCI.|true|string||
|thirdPartyCreditorId|The third party creditor's (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.|false|string||
|scheme|Can be 'Core' or 'B2B'.|true|enum (Core, B2B)|Core|
|isRecurring|Is 'true' by default.|true|boolean|true|
|umr|Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.|true|string||
|metadata|Custom information goes here.|false|string||

	
## MandateArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": 2,
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T12:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",
            "signatureDate": "2015-02-01",
            "documentId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "links": [
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ],
            "debtorId": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
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
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of mandates available.|true|integer (int32)||
|mandates||true|array[[Mandate](#mandate)]||


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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|email||false|string||
|firstName||false|string||
|lastName||false|string||
|mobile|Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.|false|string||
|address||false|[Address](#address)||
|legalEntity|Provide this field if the user is not an individual.|false|[LegalEntity](#legalentity)||
|metadata|Custom information goes here.|false|string||

	
## NewBankAccount
```json
{
    "userId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
    "metadata": "custom data"
}
```

Bank account information required to declare a new bank account.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|userId|The user's id to whom the bank account belongs.|true|string||
|holder|Holder of the bank account.|true|string||
|bic|Bank Identifier Code.|true|string||
|iban|International Bank Account Number.|true|string||
|metadata|Custom information goes here.|false|string||


## NewMandate
```json
{
    "debtorBankAccountId": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "finalCreditorId": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "useFinalCreditorSCI": false,
    "metadata": "custom data"
}
```

Information required to issue a new mandate.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|debtorBankAccountId|The debtor's bank account id.|true|string||
|finalCreditorId|The final creditor's user id.|true|string||
|useFinalCreditorSCI|If set to true, mandate will be generated with the SEPA creditor identifier (SCI) of the final creditor. Otherwise, the SCI of our PSP is taken and the creditor would appear as a third party creditor on the mandate. Remember that if you choose to generate a mandate that doens't use our PSP's SCI, you won't be able to use it to process payments on our platform.|true|string||
|metadata|Custom information goes here.|false|string||


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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|email||true|string||
|firstName||true|string||
|lastName||true|string||
|mobile|Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.|true|string||
|address||true|[Address](#address)||
|legalEntity|Provide this field if the user is not an individual.|false|[LegalEntity](#legalentity)||
|metadata|Custom information goes here.|false|string||


## NewWebhook
```json
{
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [
        "SDDMandateRequested"
    ],
    "description": "Listen to all events on users and bank accounts as well as mandate creation events."
}
```

Information required to create a new webhook.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|callbackURL|Finstack will do a POST call to this URL to send events. The URL has to be HTTPS!|true|string||
|resources|The list of resources for which events will be sent.|false|enum (BankAccount, Document, SDDMandate, User, Wallet) array||
|events|The list of event types you would like to listen to.|false|array[string]||
|description|Optional description.|false|string||


## ShortMandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "umr": "ASPECIALUMR",
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-02-01T18:00:00.000Z",
    "debtorDesignation": "Debtor SAS",
    "creditorDesignation": "Creditor SARL",
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "links": [
        {
            "rel": "Get Mandate Details",
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor",
            "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
            "verb": "GET"
        },
        {
            "rel": "Get Debtor Bank Account",
            "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "verb": "GET"
        },
        {
            "rel": "Get Creditor",
            "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
            "verb": "GET"
        },
        {
            "rel": "Get Third Party Creditor",
           "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
            "verb": "GET"
        },
        {
            "rel": "Revoke Mandate",
            "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ]
}
```

Minimal information about a mandate.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|umr|Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.|true|string||
|status|Status of the mandate.|true|enum (Canceled, PendingClientRegistration, ToBeSigned, ToBeValidated, ValidatedNotUsedYet, ValidatedUsed, Disabled, Rejected)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|debtorDesignation|Full name of the debtor whether it's an individual or legal entity. It is taken from the 'holder' field of the bank account!|true|string||
|creditorDesignation|Full name of the creditor whether it's an individual or legal entity.|true|string||
|thirdPartyCreditorDesignation|Full name of the third party creditor (if any) whether it's an individual or legal entity.|false|string||
|signatureDate|Signature date, for example '2015-01-01'.|false|string (date)||
|links|Available actions on the mandate.|true|array[[Link](#link)]||


## ShortMandateArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "updatedAt": "2015-02-01T18:00:00.000Z",
            "debtorDesignation": "Debtor SAS",
            "creditorDesignation": "Creditor SARL",
            "thirdPartyCreditorDesignation": "3rd party Company",
            "signatureDate": "2015-02-01",
            "links": [
                {
                    "rel": "Get Mandate Details",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor",
                    "href": "https://finstack.io/api/v1/users/b220221e-e461-4819-b10f-0c838c59fe82",
                    "verb": "GET"
                },
                {
                    "rel": "Get Debtor Bank Account",
                    "href": "https://finstack.io/api/v1/bank_accounts/6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
                    "verb": "GET"
                },
                {
                    "rel": "Get Creditor",
                    "href": "https://finstack.io/api/v1/users/0d919d8e-0679-4d41-a368-84e896e230ab",
                    "verb": "GET"
                },
                {
                    "rel": "Get Third Party Creditor",
                    "href": "https://finstack.io/api/v1/users/0a881459-5508-4b1b-be6f-dc512e327ee5",
                    "verb": "GET"
                },
                {
                    "rel": "Revoke Mandate",
                    "href": "https://finstack.io/api/v1/mandates/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "DELETE"
                }
            ]
        }
    ]
}
```
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of mandates available.|true|integer (int32)||
|mandates||true|array[[ShortMandate](#shortmandate)]||


## ShortUser
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "designation": "An Awesome Company",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Get User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "GET"
        },
        {
            "rel": "Update User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
            "verb": "GET"
        }
    ]
}
```

Minimal information about a user.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|designation|If the user is an individual, this field displays the first name and the last name, otherwise it would contain the name of the legal entity.|true|string||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|links|Actions available on the user.|true|array[[Link](#link)]||


## ShortUserArray
```json
{
    "offset": 0,
    "limit": 20,
    "count": 1,
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "designation": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00.000Z",
            "links": [
                {
                    "rel": "Get User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "GET"
                },
                {
                    "rel": "Update User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
                    "verb": "GET"
                }
            ]
        }
    ]
}
```
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of users available.|true|integer (int32)||
|users||true|array[[ShortUser](#shortuser)]||


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
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Find Bank Accounts of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
            "verb": "GET"
        },
        {
            "rel": "Find Mandates of User",
            "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
    "legalEntity": {
        "category": "Company",
        "name": "Acme",
        "sci": "DE98ZZZ09999999999"
    },
    "metadata": "custom data"
}
```

User information required to create a new user.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|versionNo|Version number of the object, useful to track changes through events.|true|integer (int32)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'|true|string (date-time)||
|links|Available actions on the user.|true|array[[Link](#link)]||
|email||true|string||
|firstName||true|string||
|lastName||true|string||
|mobile|Mobile phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use +33650021433 instead of 0650021433.|true|string||
|address||true|[Address](#address)||
|legalEntity|Provide this field if the user is not an individual.|false|[LegalEntity](#legalentity)||
|metadata|Custom information goes here.|false|string||


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
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
                    "verb": "PUT"
                },
                {
                    "rel": "Find Bank Accounts of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/bank_accounts",
                    "verb": "GET"
                },
                {
                    "rel": "Find Mandates of User",
                    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555/mandates",
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
            "legalEntity": {
                "category": "Company",
                "name": "Acme",
                "sci": "DE98ZZZ09999999999"
            },
            "metadata": "custom data"
        }
    ]
}
```
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|offset|Position in pagination.|true|integer (int32)||
|limit|Number of items to retrieve (100 max).|true|integer (int32)||
|count|Total number of users available.|true|integer (int32)||
|users||true|array[[User](#user)]||


## Webhook
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "createdAt": "2015-01-01T12:00:00.000Z",
    "updatedAt": "2015-01-01T12:00:00.000Z",
    "links": [
        {
            "rel": "Update Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "PUT"
        },
        {
            "rel": "Delete Webhook",
            "href": "https://finstack.io/api/v1/webhooks/341d533a-d90f-4fce-9fc0-12072f7bd555",
            "verb": "DELETE"
        }
    ],
    "callbackURL": "https://www.yourwebsite.com/webhooklistener",
    "resources": [
        "User",
        "BankAccount"
    ],
    "events": [
        "SDDMandateRequested"
    ],
    "description": "Listen to all events on users and bank accounts as well as mandate creation events."
}
```

A webhook is a subscription to listen to potentially all events in Finstack.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|links|Available actions on the web hook.|true|[[Link](#link)]||
|callbackURL|Finstack will do a POST call to this URL to send events. The URL has to be HTTPS!|true|string||
|resources|The list of resources for which events will be sent.|false|enum (BankAccount, Document, SDDMandate, User, Wallet) array||
|events|The list of event types you would like to listen to.|false|array[string]||
|description|Optional description.|false|string||


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
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|webhookId|The ID of the webhook that failed.|true|string||
|occurredAt|Timestamp at which the failure occurred in UTC, for example '2015-01-01T12:00:00.000Z'.|true|string (date-time)||
|error|Error message of the failure|true|string||
