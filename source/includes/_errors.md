# Errors

## Error

```json
{
    "code": "BankAccountForbidden",
    "message": "You cannot view bank account 02b331d1-f938-4ac4-ab40-ac287c8e8c61 because you do not manage its owner",
    "fields": {
        "bankAccountId": "02b331d1-f938-4ac4-ab40-ac287c8e8c61"
    }
}
```

All errors have the same format.
	
### Fields
|Name|Description|Required|Schema|Default|
|----|----|----|----|----|
|code| Full list is available in the [business error codes](#business-error-codes) section. |true|string||
|message| Full list is available in the [business error codes](#business-error-codes) section. |true|string||
|fields| Optional |true|string||

## HTTP Error Codes

The Finstack API uses the following HTTP error codes:

HTTP Code | Meaning
--------- | -------
400 | Bad Request -- There was an error while processing the request payload (malformed JSON, for instance)
401 | Unauthorized -- The request is not authenticated, or invalid API key
403 | Forbidden -- The request is successfully authenticated (see 401), but the action requires a different type of key
404 | Not Found -- There is no resource behind the URI
405 | Method Not Allowed -- You tried to access a resource with an invalid method (GET instead of POST for example)
406 | Not Acceptable -- You requested a format that isn't JSON
429 | Too Many Requests -- You have sent too many requests in a given amount of time ("rate limiting")
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

## Business Error Codes

The Finstack API uses the following business error codes, variables are marked with a '$' sign:

Error Code | HTTP Code | Message | Fields
---------- | --------- | ------- | ------
AccessToAggregateTypeNotAllowed | 403 -- Forbidden | Access to aggregate $aggregateId of type $aggregateType is not open to the API | aggregateType, aggregateId
AmountTooBigForDirectDebit | 400 -- Bad Request | Amount $amount is too big, the maximum amount for a direct debit is 20000€ | amount
AmountTooSmallForDirectDebit | 400 -- Bad Request | Amount $amount is too small, the minimum amount for a direct debit is 1€ | amount
BankAccountAlreadyExists | 400 -- Bad Request | Bank account $bankAccountId already exists | bankAccountId
BankAccountForbidden | 403 -- Forbidden | You cannot view bank account $bankAccountId because you do not manage its owner | bankAccountId
BankAccountNotActive | 400 -- Bad Request | Bank account $bankAccountId is not active and cannot be used in a mandate | bankAccountId
BankAccountNotFound | 404 -- Not Found | Bank account $bankAccountId was not found | bankAccountId
DeleteUserNotAllowedInProduction | 403 -- Forbidden | You cannot delete user $userId in production | userId
DirectDebitCannotBeCanceled | 400 -- Bad Request | You cannot cancel direct debit '$directDebitId' because it is in status $status | directDebitId, status
DirectDebitForbidden | 403 -- Forbidden | You cannot view direct debit $directDebitId because you do not manage one of its parties | directDebitId
DirectDebitNotFound | 404 -- Not Found | Direct debit $directDebitId was not found | directDebitId
DocumentCannotBeDeleted | 403 -- Forbidden | You cannot delete/ignore document '$fileName' because it was already sent to PSP | documentId, documentType, fileName
DocumentNotFound | 404 -- Not Found | Document $documentId was not found | documentId
InsufficientBalanceOnWallet | 400 -- Bad Request | Insufficient balance ($balance €) on wallet $walletId to pay all fees ($pendingFees €) | walletId, balance, pendingFees
InvalidCommandForMandate | 400 -- Bad Request | Received command $commandName for mandate $mandateRef in status $status | mandateId, mandateRef, commandName, status
InvalidCommandForPayment | 400 -- Bad Request | Received command $commandName for payment $paymentId in status $status | paymentId, commandName, status
InvalidJSON | 400 -- Bad Request | This error is raised anytime the JSON is invalid or any field inside it such as mobile phone, BIC or IBAN |
InvalidKeyForOperation | 403 -- Forbidden | You are not allowed to execute a $operationType operation with a $apiKeyRole API key | apiKeyId, apiKeyRole, operationType
InvalidOrNonExistentKey | 401 -- Unauthorized | Please provide a valid API key | keyHeader
InvalidOrNonExistentMandate | 400 -- Bad Request | Mandate $mandateId cannot be used either because it is in an invalid state or it does not exist | mandateId
InvalidURL | 400 -- Bad Request | String '$url' is not a valid URL or service is down | url, errorMessage
LimitShouldBeStrictlyPositive | 400 -- Bad Request | Limit ($limit) should be strictly positive | limit
MandateCannotBeCreated | 400 -- Bad Request | You cannot create a mandate because $reason | reason
MandateForbidden | 403 -- Forbidden | You cannot view mandate $mandateId because you do not manage one of its parties | mandateId
MandateIsNotToBeSigned | 400 -- Bad Request | You cannot sign mandate $mandateRef because it is in status $status | mandateId, mandateRef, status
MandateNotFound | 404 -- Not Found | Mandate $mandateId was not found | mandateId
MandateNotFoundForPayer | 404 -- Not Found | No active mandate was found for payer $email | email
MandateWasNeverSigned | 400 -- Bad Request | You cannot download the document associated to mandate $mandateRef because it was never signed | mandateId, mandateRef, status
MaximumLimitExceeded | 400 -- Bad Request | You cannot request $requestedLimit elements because it exceeds the maximum limit ($maximumLimit) | requestedLimit, maximumLimit
OffsetShouldBePositive | 400 -- Bad Request | Offset ($offset) should be positive | offset
PSPError | 400 -- Bad Request | Payment service provider error ($code): $message | code, message
SCIAndUMRAlreadyUsed | 400 -- Bad Request | A mandate already exists with SCI (SEPA Creditor Identifier) $sci and UMR (Unique Mandate Reference) $umr | sci, umr
SinceShouldBeInThePast | 400 -- Bad Request | Since ($since) should be before now ($now) | since, now
UnknownEventTypeForResourceType | 400 -- Bad Request | Event type $eventType unknown for resource type $resourceType, occurs with webhooks | eventType, resourceType
UntilShouldBeAfterSince | 400 -- Bad Request | Since ($since) should be before until ($until) | since, until
UserAlreadyExists | 400 -- Bad Request | User with email $email already exists | email 
UserForbidden | 403 -- Forbidden | You cannot view user $userId because you do not manage it | userId
UserHasNoActiveBankAccount | 400 -- Bad Request | User $name has no active bank account | userId, name
UserHasNoSCI | 400 -- Bad Request | User $name cannot be a creditor because it does not have a SEPA Creditor Identifier (SCI) | userId, name
UserNotFound | 404 -- Not Found | User $userId was not found | userId
UserWithEmailNotFound | 404 -- Not Found | User $email was not found | email
WebhookForbidden | 403 -- Forbidden | You cannot view webhook $webhookId because you do not manage its owner | webhookId
WebhookNotFound | 404 -- Not Found | Webhook $webhookId was not found | webhookId
