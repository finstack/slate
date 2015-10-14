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

//todo: migrate to html tables. after cool looking nested table
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

## Finds Bank Accounts

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

Address JSON schema.

	
### Fields
Name | Type | Default | Description
--- | --- | --- | ---
street<b title="required">&nbsp;*&nbsp;</b> | string | | Full street information including number, for example &#039;82, avenue du général Leclerc&#039;.
postCode<b title="required">&nbsp;*&nbsp;</b> | string | | For example &#039;93500&#039;.
city<b title="required">&nbsp;*&nbsp;</b> | string | | For example &#039;Paris&#039;.
country<b title="required">&nbsp;*&nbsp;</b> | string | France | For example &#039;France&#039;.

	
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

Bank account information JSON schema.

	
### Fields
Name | Type | Description
--- | --- | ---
id | string | Should be a valid UUID string.
versionNo | integer | 
status | enum | Can be 'ToBeVerified', 'Verified', 'Rejected' or 'Disabled'
createdAt | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
proof | string | ID of the uploaded document (if any) that proves that the bank account belongs to the user.
links | array[[Link](#link)] | Available actions on the bank account.
user<b title="required">&nbsp;*&nbsp;</b> | string | The user&#039;s id to whom the bank account belongs.
holder<b title="required">&nbsp;*&nbsp;</b> | string | 
bic<b title="required">&nbsp;*&nbsp;</b> | string | 
iban<b title="required">&nbsp;*&nbsp;</b> | string | 
metadata | string | Custom information goes here.

	
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
Name | Type | Description
--- | --- | ---
offset<b title="required">&nbsp;*&nbsp;</b> | integer | Position in pagination.
limit<b title="required">&nbsp;*&nbsp;</b> | integer | Number of items to retrieve (100 max).
count<b title="required">&nbsp;*&nbsp;</b> | integer | Total number of bank accounts available.
bankAccounts<b title="required">&nbsp;*&nbsp;</b> | array[[BankAccount](#bankaccount)] | 


## Error
```json
{
    "code": "string",
    "message": "string",
    "fields": "string"
}
```
	
### Fields
Name | Type | Description
--- | --- | ---
code<b title="required">&nbsp;*&nbsp;</b> | string | 
message<b title="required">&nbsp;*&nbsp;</b> | string | 
fields<b title="required">&nbsp;*&nbsp;</b> | string | 


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
Name | Type | Description
--- | --- | ---
category<b title="required">&nbsp;*&nbsp;</b> | enum | Can be 'company', 'association' or 'auto-entrepreneur'
name<b title="required">&nbsp;*&nbsp;</b> | string | 
registrationId | string | 
vatNumber | string | 
sci | [SCI](#sci) | SEPA creditor identifier, called ICS in French.


## Link
```json
{
    "rel": "Get User",
    "href": "https://finstack.io/api/v1/users/341d533a-d90f-4fce-9fc0-12072f7bd555",
    "verb": "GET"
}
```
	
### Fields
Name | Type | Description
--- | --- | ---
rel | string | Describes the action that can be conducted.
href | string | URL to execute the action.
verb | enum | HTTP verb required to execute the action. Can be 'GET', 'POST', 'PUT' and 'DELETE'.

	
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
Name | Type | Description
--- | --- | ---
id | string | Should be a valid UUID string.
versionNo | integer | 
status | string | Can be &#039;Canceled&#039;, &#039;PendingClientRegistration&#039;, &#039;ToBeSigned&#039;, &#039;ToBeValidated&#039;, &#039;ValidatedNotUsedYet&#039;, &#039;ValidatedUsed&#039;, &#039;Disabled&#039; or &#039;Rejected&#039;.
createdAt | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
debtorDesignation | string | Full name of the debtor whether it&#039;s an individual or legal entity. It is taken from the &#039;holder&#039; field of the bank account!
debtorAddress | [Address](#address) | Address of the debtor when the mandate was generated.
sci | [SCI](#sci) | SEPA Creditor Identifier.
creditorDesignation | string | Full name of the creditor whether it&#039;s an individual or legal entity.
creditorAddress | [Address](#address) | Address of the creditor when the mandate was generated.
thirdPartyCreditorDesignation | string | Full name of the third party creditor (if any) whether it&#039;s an individual or legal entity.
signatureUrl | string | This field only appears when the mandate is not signed yet. It is the link to be sent to the debtor for her to sign the mandate.
signatureDate | string | Signature date, for example &#039;2015-01-01&#039;.
document | string | ID of the signed PDF document (if any).
links | array[[Link](#link)] | Available actions on the mandate.
debtor<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s user id.
debtorBankAccount<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s bank account id.
creditor<b title="required">&nbsp;*&nbsp;</b> | string | The creditor&#039;s user id. The creditor must be a corporation that owns an SCI.
thirdPartyCreditor | string | The third party creditor&#039;s (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.
mandateType | string | Can only be &#039;SEPA&#039;.
scheme | string | Can be &#039;Core&#039; or &#039;B2B&#039;.
isRecurring | boolean | Is &#039;true&#039; by default.
umr | [UMR](#umr) | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.
clientReference | string | 
contractId | string | 
contractDescription | string | 
metadata | string | Custom information goes here.

	
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
Name | Type | Description
--- | --- | ---
offset<b title="required">&nbsp;*&nbsp;</b> | integer | Position in pagination.
limit<b title="required">&nbsp;*&nbsp;</b> | integer | Number of items to retrieve (100 max).
count<b title="required">&nbsp;*&nbsp;</b> | integer | Total number of mandates available.
mandates<b title="required">&nbsp;*&nbsp;</b> | array[[Mandate](#mandate)] | 


## MandateStatus
```json
"ValidatedUsed"
```

Can be &#039;Canceled&#039;, &#039;PendingClientRegistration&#039;, &#039;ToBeSigned&#039;, &#039;ToBeValidated&#039;, &#039;ValidatedNotUsedYet&#039;,
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

Bank account information JSON schema.

	
### Fields
Name | Type | Description
--- | --- | ---
user<b title="required">&nbsp;*&nbsp;</b> | string | The user&#039;s id to whom the bank account belongs.
holder<b title="required">&nbsp;*&nbsp;</b> | string | Holder of the bank account.
bic<b title="required">&nbsp;*&nbsp;</b> | string | Bank Identifier Code.
iban<b title="required">&nbsp;*&nbsp;</b> | string | International Bank Account Number.
metadata | string | Custom information goes here.


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
Name | Type | Default | Description
--- | --- | --- | ---
debtor<b title="required">&nbsp;*&nbsp;</b> | string | | The debtor&#039;s user id.
debtorBankAccount<b title="required">&nbsp;*&nbsp;</b> | string | | The debtor&#039;s bank account id.
creditor<b title="required">&nbsp;*&nbsp;</b> | string | | The creditor&#039;s user id. The creditor must be a corporation that owns an SCI.
thirdPartyCreditor | string | | The third party creditor&#039;s (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.
mandateType | enum | SEPA | Can only be &#039;SEPA&#039;.
scheme | enum | Core | Can be &#039;Core&#039; or &#039;B2B&#039;.
isRecurring | boolean | true | Is &#039;true&#039; by default.
umr | [UMR](#umr) | | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.
clientReference | string | |
contractId | string | |
contractDescription | string | |
metadata | string | | Custom information goes here.


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

JSON schema of user information required to create a new user.

	
### Fields
Name | Type | Description
--- | --- | ---
email<b title="required">&nbsp;*&nbsp;</b> | string | 
firstName<b title="required">&nbsp;*&nbsp;</b> | string | 
lastName<b title="required">&nbsp;*&nbsp;</b> | string | 
mobile<b title="required">&nbsp;*&nbsp;</b> | [PhoneNumber](#phonenumber) | Mobile phone number.
address<b title="required">&nbsp;*&nbsp;</b> | [Address](#address) | Address JSON schema.
phone | [PhoneNumber](#phonenumber) | Landline phone number.
legalEntity | [LegalEntity](#legalentity) | Provide this field if the user is not an individual.
metadata | string | Custom information goes here.


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
Name | Type | Description
--- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | string | Should be a valid UUID string.
umr<b title="required">&nbsp;*&nbsp;</b> | [UMR](#umr) | Unique Mandate Reference, also called RUM in French. Cannot be longer than 35 characters.
status<b title="required">&nbsp;*&nbsp;</b> | [MandateStatus](#mandatestatus) | Can be &#039;Canceled&#039;, &#039;PendingClientRegistration&#039;, &#039;ToBeSigned&#039;, &#039;ToBeValidated&#039;, &#039;ValidatedNotUsedYet&#039;, &#039;ValidatedUsed&#039;, &#039;Disabled&#039; or &#039;Rejected&#039;.
createdAt<b title="required">&nbsp;*&nbsp;</b> | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;.
debtorDesignation<b title="required">&nbsp;*&nbsp;</b> | string | Full name of the debtor whether it&#039;s an individual or legal entity. It is taken from the &#039;holder&#039; field of the bank account!
creditorDesignation<b title="required">&nbsp;*&nbsp;</b> | string | Full name of the creditor whether it&#039;s an individual or legal entity.
thirdPartyCreditorDesignation | string | Full name of the third party creditor (if any) whether it&#039;s an individual or legal entity.
signatureDate | string | Signature date, for example &#039;2015-01-01&#039;.
links<b title="required">&nbsp;*&nbsp;</b> | array[[Link](#link)] | Available actions on the mandate.

	
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
Name | Type | Description
--- | --- | ---
offset<b title="required">&nbsp;*&nbsp;</b> | integer | Position in pagination.
limit<b title="required">&nbsp;*&nbsp;</b> | integer | Number of items to retrieve (100 max).
count<b title="required">&nbsp;*&nbsp;</b> | integer | Total number of mandates available.
mandates<b title="required">&nbsp;*&nbsp;</b> | array[[ShortMandate](#shortmandate)] | 


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
Name | Type | Description
--- | --- | ---
id<b title="required">&nbsp;*&nbsp;</b> | string | Should be a valid UUID string.
designation | string | If the user is an individual, this field displays the first name and the last name, otherwise it would contain the name of the legal entity.
createdAt<b title="required">&nbsp;*&nbsp;</b> | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
links<b title="required">&nbsp;*&nbsp;</b> | array[[Link](#link)] | Link to the user.

	
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
Name | Type | Description
--- | --- | ---
offset<b title="required">&nbsp;*&nbsp;</b> | integer | Position in pagination.
limit<b title="required">&nbsp;*&nbsp;</b> | integer | Number of items to retrieve (100 max).
count<b title="required">&nbsp;*&nbsp;</b> | integer | Total number of users available.
users<b title="required">&nbsp;*&nbsp;</b> | array[[ShortUser](#shortuser)] | 


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

JSON schema of user information required to create a new user.

	
### Fields
Name | Type | Description
--- | --- | ---
id | string | Should be a valid UUID string.
versionNo | integer | 
createdAt | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
links | array[[Link](#link)] | Available actions on the user.
email<b title="required">&nbsp;*&nbsp;</b> | string | 
firstName<b title="required">&nbsp;*&nbsp;</b> | string | 
lastName<b title="required">&nbsp;*&nbsp;</b> | string | 
mobile<b title="required">&nbsp;*&nbsp;</b> | [PhoneNumber](#phonenumber) | Mobile phone number.
address<b title="required">&nbsp;*&nbsp;</b> | [Address](#address) | Address JSON schema.
phone | [PhoneNumber](#phonenumber) | Landline phone number.
legalEntity | [LegalEntity](#legalentity) | Provide this field if the user is not an individual.
metadata | string | Custom information goes here.


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
Name | Type | Description
--- | --- | ---
offset<b title="required">&nbsp;*&nbsp;</b> | integer | Position in pagination.
limit<b title="required">&nbsp;*&nbsp;</b> | integer | Number of items to retrieve (100 max).
count<b title="required">&nbsp;*&nbsp;</b> | integer | Total number of users available.
users<b title="required">&nbsp;*&nbsp;</b> | array[[User](#user)] | 
