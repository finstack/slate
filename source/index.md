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

The base URL of the API is __https://finstack.io/api/v1__.

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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "designation": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00Z",
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
limit | query | integer | 10 | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortUserArray](#shortuserarray)</td><td>An array of short users.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
        "mobile": "33612345678",
        "address": {
            "street": "82, avenue du général Leclerc",
            "postCode": "75014",
            "city": "PARIS",
            "country": "France"
        },
        "phone": "33187654321",
        "legalEntity": {
            "category": "company",
            "name": "Acme",
            "registrationId": "73282932000074",
            "vatNumber": "FR44732829320",
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
    "versionNo": "2",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T18:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
    "versionNo": "1",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
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
            "mobile": "33612345678",
            "address": {
                "street": "82, avenue du général Leclerc",
                "postCode": "75014",
                "city": "PARIS",
                "country": "France"
            },
            "phone": "33187654321",
            "legalEntity": {
                "category": "Acme",
                "name": "73282932000074",
                "registrationId": "FR44732829320",
                "vatNumber": "FR44732829320",
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
limit | query | integer | 10 | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[UserArray](#userarray)</td><td>An array of users with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get All User Events

```http
GET /api/v1/users/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "events": [
        {
            "resourceType": "User",
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "timestamp": "2015-01-01T12:00:00Z",
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

Returns all events on users since a given date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get User Event Types

```http
GET /api/v1/users/events/types HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    "UserCreated"
]
```

Returns all available event types on users ordered alphabetically.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[string]</td><td>A list of events.</td></tr> 
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
    "versionNo": "1",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
<tr><td>204</td><td>no content</td><td>User not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error</td></tr> 
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
        "mobile": "33612345678",
        "address": {
            "street": "82, avenue du général Leclerc",
            "postCode": "75014",
            "city": "PARIS",
            "country": "France"
        },
        "phone": "33187654321",
        "legalEntity": {
            "category": "company",
            "name": "Acme",
            "registrationId": "73282932000074",
            "vatNumber": "FR44732829320",
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
    "versionNo": "2",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T18:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
user<b title="required">&nbsp;*&nbsp;</b> | body | [NewUser](#newuser) | 

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>The updated user.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
            "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
            "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR1420041010050500013M02606",
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

Returns all bank accounts that belong to the specified user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the user.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccountArray](#bankaccountarray)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T18:00:00Z",
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

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortMandateArray](#shortmandatearray)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "2",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T12:00:00Z",
            "debtorDesignation": "Debtor SAS",
            "debtorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "creditorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "thirdPartyCreditorDesignation": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
            "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "mandateType": "SEPA",
            "scheme": "Core",
            "isRecurring": "true",
            "umr": "ASPECIALUMR",
            "clientReference": "CLIENT1",
            "contractId": "CONTRACT1",
            "contractDescription": "A special contract",
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
role | query | string | Filter mandates depending on role. Possible values are 'all', 'asDebtor', 'asCreditor' and 'asThirdPartyCreditor'. By default, return all mandates.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[MandateArray](#mandatearray)</td><td>An array of mandates with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": "1",
        "timestamp": "2015-01-01T12:00:00Z",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "versionNo": "1",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
            "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
            "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR1420041010050500013M02606",
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
limit | query | integer | 10 | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccountArray](#bankaccountarray)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>


## Create Bank Account

```http
POST /api/v1/bank_accounts HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "bankAccount": {
        "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "versionNo": "1",
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
    "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
    "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get All Bank Account Events

```http
GET /api/v1/bank_accounts/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "events": [
        {
            "resourceType": "BankAccount",
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "timestamp": "2015-01-01T12:00:00Z",
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

Returns all events on bank accounts since a given date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get Bank Account Event Types

```http
GET /api/v1/bank_accounts/events/types HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    "BankAccountCreated",
    "BankAccountDisabled",
    "BankAccountUpdatedByPSP"
]
```

Returns all available event types on bank accounts ordered alphabetically.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[string]</td><td>A list of events.</td></tr> 
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
    "versionNo": "1",
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
    "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
    "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
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
<tr><td>204</td><td>no content</td><td>Bank account not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "versionNo": "2",
    "status": "Disabled",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T18:00:00Z",
    "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
    "links": [],
    "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
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
<tr><td>204</td><td>no content</td><td>Bank account not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": "1",
        "timestamp": "2015-01-01T12:00:00Z",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
            "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
            "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR1420041010050500013M02606",
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

Finds all bank accounts that belong to the authenticated user.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccountArray](#bankaccountarray)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T12:00:00Z",
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

Returns mandates that belong to users that you manage, or mandates that you created.


### Parameters
Name | In | Type | Default | Description
--- | --- | --- | --- | ---
offset | query | integer | 0 | Offset the list of returned results by this amount.
limit | query | integer | 10 | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[ShortMandateArray](#shortmandatearray)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Create Mandate

```http
POST /api/v1/mandates HTTP/1.1
Content-Type: application/json
X-Auth-Token: myapikeyvalue

{
    "mandate": {
        "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
        "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
        "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
        "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
        "mandateType": "SEPA",
        "scheme": "Core",
        "isRecurring": "true",
        "umr": "ASPECIALUMR",
        "clientReference": "CLIENT1",
        "contractId": "CONTRACT1",
        "contractDescription": "A special contract",
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
    "versionNo": "1",
    "status": "ToBeSigned",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
    "debtorDesignation": "Debtor SAS",
    "debtorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "creditorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureUrl": "https://finstack.io/admin/mandate/a550693f-9743-4e79-bf92-24a023bb81d5/sign/0893af5d-94ef-45e2-89c8-b5c1658d8503",
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
    "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "mandateType": "SEPA",
    "scheme": "Core",
    "isRecurring": "true",
    "umr": "ASPECIALUMR",
    "clientReference": "CLIENT1",
    "contractId": "CONTRACT1",
    "contractDescription": "A special contract",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "2",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T12:00:00Z",
            "debtorDesignation": "Debtor SAS",
            "debtorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "creditorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "thirdPartyCreditorDesignation": "3rd party Company",            
            "signatureDate": "2015-02-01",
            "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
            "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "mandateType": "SEPA",
            "scheme": "Core",
            "isRecurring": "true",
            "umr": "ASPECIALUMR",
            "clientReference": "CLIENT1",
            "contractId": "CONTRACT1",
            "contractDescription": "A special contract",
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

Returns mandates that belong to users that you manage, or mandates that you created.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[MandateArray](#mandatearray)</td><td>An array of mandates with details.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get All Mandate Events

```http
GET /api/v1/mandates/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "events": [
        {
            "resourceType": "SDDMandate",
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "timestamp": "2015-01-01T12:00:00Z",
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

Returns all events on mandates since a given date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get Mandate Event Types

```http
GET /api/v1/mandates/events/types HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    "ClientRegistered",
    "SDDMandateCanceled",
    "SDDMandateRejectedByPSP",
    "SDDMandateRequested",
    "SDDMandateRevoked",
    "SDDMandateSigned",
    "SDDMandateUsedByPSP",
    "SDDMandateValidatedByPSP",
    "SignedDocumentStored"
]
```

Returns all available event types on mandates ordered alphabetically.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[string]</td><td>A list of events.</td></tr> 
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
    "versionNo": "2",
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-02-01T12:00:00Z",
    "debtorDesignation": "Debtor SAS",
    "debtorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "creditorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "mandateType": "SEPA",
    "scheme": "Core",
    "isRecurring": "true",
    "umr": "ASPECIALUMR",
    "clientReference": "CLIENT1",
    "contractId": "CONTRACT1",
    "contractDescription": "A special contract",
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
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
    "versionNo": "3",
    "status": "Disabled",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-02-01T12:00:00Z",
    "debtorDesignation": "Debtor SAS",
    "debtorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "creditorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "mandateType": "SEPA",
    "scheme": "Core",
    "isRecurring": "true",
    "umr": "ASPECIALUMR",
    "clientReference": "CLIENT1",
    "contractId": "CONTRACT1",
    "contractDescription": "A special contract",
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

Cancels the mandate if it isn&#039;t signed yet or revokes it if it is signed.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the bank account.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandate](#mandate)</td><td>Mandate canceled or revoked.</td></tr> 
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get Signed Mandate in PDF

```http
GET /api/v1/mandates/{id}/pdf HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
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

Returns signed mandate (if any) with the specified id in PDF.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | path | string | UUID of the mandate.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>no content</td><td>Signed mandate file in PDF format.</td></tr> 
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
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
        "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
        "versionNo": "1",
        "timestamp": "2015-01-01T12:00:00Z",
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
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

# Events
## Get All Events

```http
GET /api/v1/events HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "events": [
        {
            "resourceType": "SDDMandate",
            "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "timestamp": "2015-01-01T12:00:00Z",
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

Returns all events since a given date.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
since<b title="required">&nbsp;*&nbsp;</b> | query | string | Return all events after given timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
offset | query | integer | Offset the list of returned results by this amount.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.

### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[EventArray](#eventarray)</td><td>A list of events ordered chronologically.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get All Event Types

```http
GET /api/v1/events/types HTTP/1.1
X-Auth-Token: myapikeyvalue
```

> HTTP response example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    "BankAccountCreated",
    "BankAccountDisabled",
    "BankAccountUpdatedByPSP"
    "ClientRegistered",
    "SDDMandateCanceled",
    "SDDMandateRejectedByPSP",
    "SDDMandateRequested",
    "SDDMandateRevoked",
    "SDDMandateSigned",
    "SDDMandateUsedByPSP",
    "SDDMandateValidatedByPSP",
    "SignedDocumentStored",
    "UserCreated"
]
```

Returns all available event types ordered alphabetically.


### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>array[string]</td><td>A list of events.</td></tr> 
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
    "versionNo": "1",
    "status": "Verified",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
    "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
    "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
    "holder": "Marc Dupont",
    "bic": "SOGEFRPPXXX",
    "iban": "FR1420041010050500013M02606",
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
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00Z'.|false|string (date-time)||
|proof|ID of the uploaded document (if any) that proves that the bank account belongs to the user.|false|string||
|links|Available actions on the bank account.|true|array[[Link](#link)]||
|user|The user's id to whom the bank account belongs.|true|string||
|holder|Holder of the bank account.|true|string||
|bic|Bank Identifier Code.|true|string||
|iban|International Bank Account Number.|true|string||
|metadata|Custom information goes here.|false|string||

	
## BankAccountArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "bankAccounts": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "status": "Verified",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
            "proof": "01e55840-149c-48dc-aadc-0448e066a9f5",
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
            "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
            "holder": "Marc Dupont",
            "bic": "SOGEFRPPXXX",
            "iban": "FR1420041010050500013M02606",
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
    "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "1",
    "timestamp": "2015-01-01T12:00:00Z",
    "commandId": "b220221e-e461-4819-b10f-0c838c59fe82",
    "eventType": "UserCreated"
}
```

An object that describes an event that occurred to a resource.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|resourceType|Type of resource that the event applies to.|true|enum (BankAccount, Document, SDDMandate, User, Wallet)||
|resourceId|ID of the resource affected by the event.|true|string||
|versionNo|Version number of the resource after the event occurs.|true|integer (int32)||
|timestamp|Event timestamp in UTC, for example '2015-01-01T12:00:00Z'.|true|string (date-time)||
|commandId|ID of the command that triggered the event.|true|string||
|eventType|Type of the event.|true|string||
	
## EventArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "events": [
        {
             "resourceType": "User",
             "resourceId": "341d533a-d90f-4fce-9fc0-12072f7bd555",
             "versionNo": "1",
             "timestamp": "2015-01-01T12:00:00Z",
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
    "category": "company",
    "name": "Acme",
    "registrationId": "73282932000074",
    "vatNumber": "FR44732829320",
    "sci": "DE98ZZZ09999999999"
}
```

Information about all types of corporation such as companies, associations...

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|category|Type of legal entity.|true|enum (company, association, auto-entrepreneur)|company|
|name||true|string||
|registrationId||false|string||
|vatNumber||false|string||
|sci|SEPA creditor identifier, called ICS in French.|false|[SCI](#sci)||


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
    "versionNo": "2",
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-02-01T12:00:00Z",
    "debtorDesignation": "Debtor SAS",
    "debtorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "sci": "DE98ZZZ09999999999",
    "creditorDesignation": "Creditor SARL",
    "creditorAddress": {
        "street": "13, rue du paradis",
        "postCode": "54000",
        "city": "NANCY",
        "country": "France"
    },
    "thirdPartyCreditorDesignation": "3rd party Company",
    "signatureDate": "2015-02-01",
    "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
    "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "mandateType": "SEPA",
    "scheme": "Core",
    "isRecurring": "true",
    "umr": "ASPECIALUMR",
    "clientReference": "CLIENT1",
    "contractId": "CONTRACT1",
    "contractDescription": "A special contract",
    "metadata": "custom data"
}
```

Information required to issue a new mandate.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|id|Should be a valid UUID string.|true|string||
|versionNo|Version number of the object, useful to track changes through events.|true|integer (int32)||
|status|Status of the mandate.|true|[MandateStatus](#mandatestatus)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00Z'.|false|string (date-time)||
|debtorDesignation|Full name of the debtor whether it's an individual or legal entity. It is taken from the 'holder' field of the bank account!|true|string||
|debtorAddress|Address of the debtor when the mandate was generated.|true|[Address](#address)||
|sci|SEPA Creditor Identifier.|true|[SCI](#sci)||
|creditorDesignation|Full name of the creditor whether it's an individual or legal entity.|true|string||
|creditorAddress|Address of the creditor when the mandate was generated.|true|[Address](#address)||
|thirdPartyCreditorDesignation|Full name of the third party creditor (if any) whether it's an individual or legal entity.|false|string||
|signatureUrl|This field only appears when the mandate is not signed yet. It is the link to be sent to the debtor for her to sign the mandate.|false|string||
|signatureDate|Signature date, for example '2015-01-01'.|false|string (date)||
|document|ID of the signed PDF document (if any).|false|string||
|links|Available actions on the mandate.|true|array[[Link](#link)]||
|debtor|The debtor's user id.|true|string||
|debtorBankAccount|The debtor's bank account id.|true|string||
|creditor|The creditor's user id. The creditor must be a corporation that owns an SCI.|true|string||
|thirdPartyCreditor|The third party creditor's (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.|false|string||
|mandateType|Can only be 'SEPA'.|false|enum (SEPA)|SEPA|
|scheme|Can be 'Core' or 'B2B'.|false|enum (Core, B2B)|Core|
|isRecurring|Is 'true' by default.|false|boolean|true|
|umr||false|[UMR](#umr)||
|clientReference||false|string||
|contractId||false|string||
|contractDescription||false|string||
|metadata|Custom information goes here.|false|string||

	
## MandateArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "2",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T12:00:00Z",
            "debtorDesignation": "Debtor SAS",
            "debtorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "sci": "DE98ZZZ09999999999",
            "creditorDesignation": "Creditor SARL",
            "creditorAddress": {
                "street": "13, rue du paradis",
                "postCode": "54000",
                "city": "NANCY",
                "country": "France"
            },
            "thirdPartyCreditorDesignation": "3rd party Company",
            "signatureUrl": "string",
            "signatureDate": "2015-02-01",
            "document": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
            "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
            "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
            "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
            "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
            "mandateType": "SEPA",
            "scheme": "Core",
            "isRecurring": "true",
            "umr": "ASPECIALUMR",
            "clientReference": "CLIENT1",
            "contractId": "CONTRACT1",
            "contractDescription": "A special contract",
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


## MandateStatus
```json
"ValidatedUsed"
```

Status of the mandate. Can be &#039;Canceled&#039;, &#039;PendingClientRegistration&#039;, &#039;ToBeSigned&#039;, &#039;ToBeValidated&#039;, &#039;ValidatedNotUsedYet&#039;,
&#039;ValidatedUsed&#039;, &#039;Disabled&#039; or &#039;Rejected&#039;.

	
## NewBankAccount
```json
{
    "user": "02b331d1-f938-4ac4-ab40-ac287c8e8c61",
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
|user|The user's id to whom the bank account belongs.|true|string||
|holder|Holder of the bank account.|true|string||
|bic|Bank Identifier Code.|true|string||
|iban|International Bank Account Number.|true|string||
|metadata|Custom information goes here.|false|string||


## NewMandate
```json
{
    "debtor": "b220221e-e461-4819-b10f-0c838c59fe82",
    "debtorBankAccount": "6f83ebf1-6e4d-40f6-bff5-5f046a93560a",
    "creditor": "0d919d8e-0679-4d41-a368-84e896e230ab",
    "thirdPartyCreditor": "0a881459-5508-4b1b-be6f-dc512e327ee5",
    "mandateType": "SEPA",
    "scheme": "Core",
    "isRecurring": "true",
    "umr": "ASPECIALUMR",
    "clientReference": "CLIENT1",
    "contractId": "CONTRACT1",
    "contractDescription": "A special contract",
    "metadata": "custom data"
}
```

Information required to issue a new mandate.

	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|debtor|The debtor's user id.|true|string||
|debtorBankAccount|The debtor's bank account id.|true|string||
|creditor|The creditor's user id. The creditor must be a corporation that owns an SCI.|true|string||
|thirdPartyCreditor|The third party creditor's (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.|false|string||
|mandateType|Can only be 'SEPA'.|false|enum (SEPA)|SEPA|
|scheme|Can be 'Core' or 'B2B'.|false|enum (Core, B2B)|Core|
|isRecurring|Is 'true' by default.|false|boolean|true|
|umr||false|[UMR](#umr)||
|clientReference||false|string||
|contractId||false|string||
|contractDescription||false|string||
|metadata|Custom information goes here.|false|string||


## NewUser
```json
{
    "email": "user@example.com",
    "firstName": "Marc",
    "lastName": "Dupont",
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
|mobile|Mobile phone number.|true|[PhoneNumber](#phonenumber)||
|address||true|[Address](#address)||
|phone|Landline phone number.|false|[PhoneNumber](#phonenumber)||
|legalEntity|Provide this field if the user is not an individual.|false|[LegalEntity](#legalentity)||
|metadata|Custom information goes here.|false|string||


## PhoneNumber
```json
"33612345678"
```

Phone number respecting the <a href="https://en.wikipedia.org/wiki/MSISDN">MSISDN</a> format. For example use 33650021433 instead of 0650021433.


## SCI
```json
"DE98ZZZ09999999999"
```

SEPA creditor identifier, called ICS in French. Maximum length is 35 characters.


## ShortMandate
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "umr": "ASPECIALUMR",
    "status": "ValidatedNotUsedYet",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-02-01T18:00:00Z",
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
|umr|Unique Mandate Reference, also called RUM in French.|true|[UMR](#umr)||
|status|Status of the mandate.|true|[MandateStatus](#mandatestatus)||
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00Z'.|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00Z'.|false|string (date-time)||
|debtorDesignation|Full name of the debtor whether it's an individual or legal entity. It is taken from the 'holder' field of the bank account!|true|string||
|creditorDesignation|Full name of the creditor whether it's an individual or legal entity.|true|string||
|thirdPartyCreditorDesignation|Full name of the third party creditor (if any) whether it's an individual or legal entity.|false|string||
|signatureDate|Signature date, for example '2015-01-01'.|false|string (date)||
|links|Available actions on the mandate.|true|array[[Link](#link)]||


## ShortMandateArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "mandates": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "umr": "ASPECIALUMR",
            "status": "ValidatedNotUsedYet",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-02-01T18:00:00Z",
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
    "createdAt": "2015-01-01T12:00:00Z",
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
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00Z'|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00Z'.|false|string (date-time)||
|links|Actions available on the user.|true|array[[Link](#link)]||


## ShortUserArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "designation": "An Awesome Company",
            "createdAt": "2015-01-01T12:00:00Z",
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


## UMR
```json
"MYSPECIALUMR"
```

Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.


## User
```json
{
    "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
    "versionNo": "1",
    "createdAt": "2015-01-01T12:00:00Z",
    "updatedAt": "2015-01-01T12:00:00Z",
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
    "mobile": "33612345678",
    "address": {
        "street": "82, avenue du général Leclerc",
        "postCode": "75014",
        "city": "PARIS",
        "country": "France"
    },
    "phone": "33187654321",
    "legalEntity": {
        "category": "company",
        "name": "Acme",
        "registrationId": "73282932000074",
        "vatNumber": "FR44732829320",
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
|createdAt|Creation timestamp in UTC, for example '2015-01-01T12:00:00Z'|true|string (date-time)||
|updatedAt|Last update timestamp in UTC, for example '2015-01-01T12:00:00Z'|false|string (date-time)||
|links|Available actions on the user.|true|array[[Link](#link)]||
|email||true|string||
|firstName||true|string||
|lastName||true|string||
|mobile|Mobile phone number.|true|[PhoneNumber](#phonenumber)||
|address||true|[Address](#address)||
|phone|Landline phone number.|false|[PhoneNumber](#phonenumber)||
|legalEntity|Provide this field if the user is not an individual.|false|[LegalEntity](#legalentity)||
|metadata|Custom information goes here.|false|string||


## UserArray
```json
{
    "offset": "0",
    "limit": "10",
    "count": "1",
    "users": [
        {
            "id": "341d533a-d90f-4fce-9fc0-12072f7bd555",
            "versionNo": "1",
            "createdAt": "2015-01-01T12:00:00Z",
            "updatedAt": "2015-01-01T12:00:00Z",
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
            "mobile": "33612345678",
            "address": {
                "street": "82, avenue du général Leclerc",
                "postCode": "75014",
                "city": "PARIS",
                "country": "France"
            },
            "phone": "33187654321",
            "legalEntity": {
                "category": "company",
                "name": "Acme",
                "registrationId": "73282932000074",
                "vatNumber": "FR44732829320",
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
