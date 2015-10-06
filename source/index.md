---
title: Finstack API Reference

language_tabs:
  - shell: cURL

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Finstack API! You can use our API to access Finstack API endpoints, which can get information on various users, mandates, and direct debits in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](http://github.com/tripit/slate).

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Users

## Get All Users

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve


> ### Consumes  
> `application/json`  

> ### Produces
> `application/json`

### Authorization: API Key
Type | Name | In | Description
--- | --- | --- | ---
apiKey | X-Auth-Token | header | API Key that you created in your admin interface



## Find users

```http
GET /api/v1/users HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "users": [
        {
            "id": "string",
            "versionNo": "integer",
            "createdAt": "string",
            "updatedAt": "string",
            "email": "string",
            "firstName": "string",
            "lastName": "string",
            "mobile": "string",
            "address": {
                "street": "string",
                "postCode": "string",
                "city": "string",
                "country": "string"
            },
            "phone": "string",
            "legalEntity": {
                "category": "string",
                "name": "string",
                "registrationId": "string",
                "vatNumber": "string",
                "sci": "string"
            },
            "metadata": "string"
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
Name | In | Type | Description
--- | --- | --- | ---
offset | query | integer | Offset the list of returned results by this amount. Default is zero.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Users](#users)</td><td>An array of users.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>
## Create user

```http
POST /api/v1/users HTTP/1.1
Content-Type: application/json

{
    "user": {
        "email": "string",
        "firstName": "string",
        "lastName": "string",
        "mobile": "string",
        "address": {
            "street": "string",
            "postCode": "string",
            "city": "string",
            "country": "string"
        },
        "phone": "string",
        "legalEntity": {
            "category": "string",
            "name": "string",
            "registrationId": "string",
            "vatNumber": "string",
            "sci": "string"
        },
        "metadata": "string"
    }
}
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
}
```
```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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
Name | In | Type | Description
--- | --- | --- | ---
isUpdateAllowed | query | boolean | This parameter allows to update an existing user than throw an error if she already exists.
Only use it when your system cannot garanty the unicity of user accounts and their associated emails.

user<b title="required">&nbsp;*&nbsp;</b> | body | [NewUser](#newuser) | 
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>The updated user, if `isUpdateAllowed` is set to `true`, and the email provided belongs to an existing user.
</td></tr> 
<tr><td>201</td><td>[User](#user)</td><td>The created user.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get user

```http
GET /api/v1/users/{id} HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>User found.</td></tr> 
<tr><td>204</td><td>no content</td><td>User not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error</td></tr> 
</table>
## Update user

```http
PUT /api/v1/users/{id} HTTP/1.1
Content-Type: application/json

{
    "user": {
        "email": "string",
        "firstName": "string",
        "lastName": "string",
        "mobile": "string",
        "address": {
            "street": "string",
            "postCode": "string",
            "city": "string",
            "country": "string"
        },
        "phone": "string",
        "legalEntity": {
            "category": "string",
            "name": "string",
            "registrationId": "string",
            "vatNumber": "string",
            "sci": "string"
        },
        "metadata": "string"
    }
}
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[User](#user)</td><td>The updated user.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Find bank accounts of user

```http
GET /api/v1/users/{id}/bank_accounts HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "bankAccounts": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "proof": "string",
            "user": "string",
            "holder": "string",
            "bic": "string",
            "iban": "string",
            "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccounts](#bankaccounts)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Find mandates of user

```http
GET /api/v1/users/{id}/mandates HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "mandates": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "signatureUrl": "string",
            "document": "string",
            "signatureDate": "string",
            "fee": "number",
            "debtor": "string",
            "debtorBankAccount": "string",
            "creditor": "string",
            "thirdPartyCreditor": "string",
            "mandateType": "string",
            "scheme": "string",
            "isRecurring": "boolean",
            "umr": "string",
            "clientReference": "string",
            "contractId": "string",
            "contractDescription": "string",
            "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandates](#mandates)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get my user profile

```http
GET /api/v1/users/me HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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

## Finds bank accounts

```http
GET /api/v1/bank_accounts HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "bankAccounts": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "proof": "string",
            "user": "string",
            "holder": "string",
            "bic": "string",
            "iban": "string",
            "metadata": "string"
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
Name | In | Type | Description
--- | --- | --- | ---
offset | query | integer | Offset the list of returned results by this amount. Default is zero.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccounts](#bankaccounts)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>
## Create bank account

```http
POST /api/v1/bank_accounts HTTP/1.1
Content-Type: application/json

{
    "bankAccount": {
        "user": "string",
        "holder": "string",
        "bic": "string",
        "iban": "string",
        "metadata": "string"
    }
}
```
```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "proof": "string",
    "user": "string",
    "holder": "string",
    "bic": "string",
    "iban": "string",
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>201</td><td>[BankAccount](#bankaccount)</td><td>URL of the created bank account.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get bank account

```http
GET /api/v1/bank_accounts/{id} HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "proof": "string",
    "user": "string",
    "holder": "string",
    "bic": "string",
    "iban": "string",
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccount](#bankaccount)</td><td>Bank account found.</td></tr> 
<tr><td>204</td><td>no content</td><td>Bank account not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>
## Disable bank account

```http
DELETE /api/v1/bank_accounts/{id} HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "proof": "string",
    "user": "string",
    "holder": "string",
    "bic": "string",
    "iban": "string",
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[BankAccount](#bankaccount)</td><td>Disabled bank account.</td></tr> 
<tr><td>204</td><td>no content</td><td>Bank account not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Find my bank accounts

```http
GET /api/v1/bank_accounts/me HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "bankAccounts": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "proof": "string",
            "user": "string",
            "holder": "string",
            "bic": "string",
            "iban": "string",
            "metadata": "string"
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
<tr><td>200</td><td>[BankAccounts](#bankaccounts)</td><td>An array of bank accounts.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

# Mandates

## Find mandates

```http
GET /api/v1/mandates HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "mandates": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "signatureUrl": "string",
            "document": "string",
            "signatureDate": "string",
            "fee": "number",
            "debtor": "string",
            "debtorBankAccount": "string",
            "creditor": "string",
            "thirdPartyCreditor": "string",
            "mandateType": "string",
            "scheme": "string",
            "isRecurring": "boolean",
            "umr": "string",
            "clientReference": "string",
            "contractId": "string",
            "contractDescription": "string",
            "metadata": "string"
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
offset | query | integer | Offset the list of returned results by this amount. Default is zero.
limit | query | integer | Number of items to retrieve. Default is 10, maximum is 100.
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandates](#mandates)</td><td>An array of mandates.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get mandate

```http
GET /api/v1/mandates/{id} HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "signatureUrl": "string",
    "document": "string",
    "signatureDate": "string",
    "fee": "number",
    "debtor": "string",
    "debtorBankAccount": "string",
    "creditor": "string",
    "thirdPartyCreditor": "string",
    "mandateType": "string",
    "scheme": "string",
    "isRecurring": "boolean",
    "umr": "string",
    "clientReference": "string",
    "contractId": "string",
    "contractDescription": "string",
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandate](#mandate)</td><td>Mandate found.</td></tr> 
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>
## Cancel or revoke mandate

```http
DELETE /api/v1/mandates/{id} HTTP/1.1
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "signatureUrl": "string",
    "document": "string",
    "signatureDate": "string",
    "fee": "number",
    "debtor": "string",
    "debtorBankAccount": "string",
    "creditor": "string",
    "thirdPartyCreditor": "string",
    "mandateType": "string",
    "scheme": "string",
    "isRecurring": "boolean",
    "umr": "string",
    "clientReference": "string",
    "contractId": "string",
    "contractDescription": "string",
    "metadata": "string"
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>[Mandate](#mandate)</td><td>Mandate canceled or revoked.</td></tr> 
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>

## Get signed mandate in pdf

```http
GET /api/v1/mandates/{id}/pdf HTTP/1.1
```
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
//todo: migrate to html tables. after cool looking nested table
### Responses
<span comment="workaround for markdown processing in table"></span>
<table>
<tr><th>Http code</th><th>Type</th><th>Description</th></tr>
<tr><td>200</td><td>no content</td><td>Signed mandate file in PDF format.</td></tr> 
<tr><td>204</td><td>no content</td><td>Mandate not found.</td></tr> 
<tr><td>400</td><td>[Error](#error)</td><td>Unexpected error.</td></tr> 
</table>


# Models
## SCI
```json
"string"
```

SEPA creditor identifier, called ICS in French.

	
### Fields
Name | Type | Description
--- | --- | ---

	
## Address
```json
{
    "street": "string",
    "postCode": "string",
    "city": "string",
    "country": "string"
}
```

Address JSON schema.

	
### Fields
Name | Type | Description
--- | --- | ---
street<b title="required">&nbsp;*&nbsp;</b> | string | Full street information including number, for example &#039;82, avenue du général Leclerc&#039;.
postCode<b title="required">&nbsp;*&nbsp;</b> | string | For example &#039;93500&#039;.
city<b title="required">&nbsp;*&nbsp;</b> | string | For example &#039;Paris&#039;.
country<b title="required">&nbsp;*&nbsp;</b> | string | For example &#039;France&#039;.

	
## PhoneNumber
```json
"string"
```

Phone number respecting the &lt;a href=&quot;https://en.wikipedia.org/wiki/MSISDN&quot;&gt;MSISDN&lt;/a&gt; format. For example use 33650021433 instead of 0650021433.

	
### Fields
Name | Type | Description
--- | --- | ---

	
## LegalEntity
```json
{
    "category": "string",
    "name": "string",
    "registrationId": "string",
    "vatNumber": "string",
    "sci": "string"
}
```

Information about all types of corporation such as companies, associations...

	
### Fields
Name | Type | Description
--- | --- | ---
category<b title="required">&nbsp;*&nbsp;</b> | string | 
name<b title="required">&nbsp;*&nbsp;</b> | string | 
registrationId | string | 
vatNumber | string | 
sci | [SCI](#sci) | SEPA creditor identifier, called ICS in French.

	
## NewUser
```json
{
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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

	
## User
```json
{
    "id": "string",
    "versionNo": "integer",
    "createdAt": "string",
    "updatedAt": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "mobile": "string",
    "address": {
        "street": "string",
        "postCode": "string",
        "city": "string",
        "country": "string"
    },
    "phone": "string",
    "legalEntity": {
        "category": "string",
        "name": "string",
        "registrationId": "string",
        "vatNumber": "string",
        "sci": "string"
    },
    "metadata": "string"
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
email<b title="required">&nbsp;*&nbsp;</b> | string | 
firstName<b title="required">&nbsp;*&nbsp;</b> | string | 
lastName<b title="required">&nbsp;*&nbsp;</b> | string | 
mobile<b title="required">&nbsp;*&nbsp;</b> | [PhoneNumber](#phonenumber) | Mobile phone number.
address<b title="required">&nbsp;*&nbsp;</b> | [Address](#address) | Address JSON schema.
phone | [PhoneNumber](#phonenumber) | Landline phone number.
legalEntity | [LegalEntity](#legalentity) | Provide this field if the user is not an individual.
metadata | string | Custom information goes here.

	
## Users
```json
{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "users": [
        {
            "id": "string",
            "versionNo": "integer",
            "createdAt": "string",
            "updatedAt": "string",
            "email": "string",
            "firstName": "string",
            "lastName": "string",
            "mobile": "string",
            "address": {
                "street": "string",
                "postCode": "string",
                "city": "string",
                "country": "string"
            },
            "phone": "string",
            "legalEntity": {
                "category": "string",
                "name": "string",
                "registrationId": "string",
                "vatNumber": "string",
                "sci": "string"
            },
            "metadata": "string"
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

	
## NewBankAccount
```json
{
    "user": "string",
    "holder": "string",
    "bic": "string",
    "iban": "string",
    "metadata": "string"
}
```

Bank account information JSON schema.

	
### Fields
Name | Type | Description
--- | --- | ---
user<b title="required">&nbsp;*&nbsp;</b> | string | The user&#039;s id to whom the bank account belongs.
holder<b title="required">&nbsp;*&nbsp;</b> | string | 
bic<b title="required">&nbsp;*&nbsp;</b> | string | 
iban<b title="required">&nbsp;*&nbsp;</b> | string | 
metadata | string | Custom information goes here.

	
## BankAccount
```json
{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "proof": "string",
    "user": "string",
    "holder": "string",
    "bic": "string",
    "iban": "string",
    "metadata": "string"
}
```

Bank account information JSON schema.

	
### Fields
Name | Type | Description
--- | --- | ---
id | string | Should be a valid UUID string.
versionNo | integer | 
status | string | 
createdAt | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
proof | string | ID of the uploaded document (if any) that proves that the bank account belongs to the user.
user<b title="required">&nbsp;*&nbsp;</b> | string | The user&#039;s id to whom the bank account belongs.
holder<b title="required">&nbsp;*&nbsp;</b> | string | 
bic<b title="required">&nbsp;*&nbsp;</b> | string | 
iban<b title="required">&nbsp;*&nbsp;</b> | string | 
metadata | string | Custom information goes here.

	
## BankAccounts
```json
{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "bankAccounts": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "proof": "string",
            "user": "string",
            "holder": "string",
            "bic": "string",
            "iban": "string",
            "metadata": "string"
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

	
## UMR
```json
"string"
```

Unique Mandate Reference, also called RUM in French.

	
### Fields
Name | Type | Description
--- | --- | ---

	
## NewMandate
```json
{
    "debtor": "string",
    "debtorBankAccount": "string",
    "creditor": "string",
    "thirdPartyCreditor": "string",
    "mandateType": "string",
    "scheme": "string",
    "isRecurring": "boolean",
    "umr": "string",
    "clientReference": "string",
    "contractId": "string",
    "contractDescription": "string",
    "metadata": "string"
}
```

Information required to issue a new mandate.

	
### Fields
Name | Type | Description
--- | --- | ---
debtor<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s user id.
debtorBankAccount<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s bank account id.
creditor<b title="required">&nbsp;*&nbsp;</b> | string | The creditor&#039;s user id. The creditor must be a corporation that owns an SCI.
thirdPartyCreditor | string | The third party creditor&#039;s (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.
mandateType | string | 
scheme | string | 
isRecurring | boolean | 
umr | [UMR](#umr) | Unique Mandate Reference, also called RUM in French.
clientReference | string | 
contractId | string | 
contractDescription | string | 
metadata | string | Custom information goes here.

	
## Mandate
```json
{
    "id": "string",
    "versionNo": "integer",
    "status": "string",
    "createdAt": "string",
    "updatedAt": "string",
    "signatureUrl": "string",
    "document": "string",
    "signatureDate": "string",
    "fee": "number",
    "debtor": "string",
    "debtorBankAccount": "string",
    "creditor": "string",
    "thirdPartyCreditor": "string",
    "mandateType": "string",
    "scheme": "string",
    "isRecurring": "boolean",
    "umr": "string",
    "clientReference": "string",
    "contractId": "string",
    "contractDescription": "string",
    "metadata": "string"
}
```

Information required to issue a new mandate.

	
### Fields
Name | Type | Description
--- | --- | ---
id | string | Should be a valid UUID string.
versionNo | integer | 
status | string | 
createdAt | string | Creation timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
updatedAt | string | Last update timestamp in UTC, for example &#039;2015-01-01T12:00:00Z&#039;
signatureUrl | string | 
document | string | ID of the signed PDF document (if any).
signatureDate | string | 
fee | number | 
debtor<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s user id.
debtorBankAccount<b title="required">&nbsp;*&nbsp;</b> | string | The debtor&#039;s bank account id.
creditor<b title="required">&nbsp;*&nbsp;</b> | string | The creditor&#039;s user id. The creditor must be a corporation that owns an SCI.
thirdPartyCreditor | string | The third party creditor&#039;s (or true creditor) user id. This field is used when the creditor is a PSP that has a wallet for the real creditor.
mandateType | string | 
scheme | string | 
isRecurring | boolean | 
umr | [UMR](#umr) | Unique Mandate Reference, also called RUM in French.
clientReference | string | 
contractId | string | 
contractDescription | string | 
metadata | string | Custom information goes here.

	
## Mandates
```json
{
    "offset": "integer",
    "limit": "integer",
    "count": "integer",
    "mandates": [
        {
            "id": "string",
            "versionNo": "integer",
            "status": "string",
            "createdAt": "string",
            "updatedAt": "string",
            "signatureUrl": "string",
            "document": "string",
            "signatureDate": "string",
            "fee": "number",
            "debtor": "string",
            "debtorBankAccount": "string",
            "creditor": "string",
            "thirdPartyCreditor": "string",
            "mandateType": "string",
            "scheme": "string",
            "isRecurring": "boolean",
            "umr": "string",
            "clientReference": "string",
            "contractId": "string",
            "contractDescription": "string",
            "metadata": "string"
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
