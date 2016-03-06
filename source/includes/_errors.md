# HTTP Error Codes

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

# Business Error Codes

The Finstack API uses the following business error codes, variables are marked with a '$' sign:

Error Code | HTTP Code | Message
---------- | --------- | -------
BankAccountForbidden | 403 -- Forbidden | You cannot view bank account $bankAccountId because you do not manage its owner
BankAccountNotActive | 400 -- Bad Request | Bank account $bankAccountId is not active and cannot be used in a mandate
BankAccountNotFound | 404 -- Not Found | Bank account $bankAccountId was not found
DeleteUserNotAllowedInProduction | 403 -- Forbidden | You cannot delete user $userId in production
DocumentNotFound | 404 -- Not Found | Document $documentId was not found
InsufficientBalanceOnWallet | 400 -- Bad Request | Insufficient balance ($balance €) on wallet $walletId to pay all fees ($pendingFees €)
InvalidCommandForMandate | 400 -- Bad Request | Received command $commandName for mandate $mandateRef in status $status
InvalidCommandForPayment | 400 -- Bad Request | Received command $commandName for payment $paymentId in status $status
InvalidKeyForOperation | 403 -- Forbidden | You are not allowed to execute a $operationType operation with a $apiKeyRole API key
InvalidOrNonExistentKey | 401 -- Unauthorized | Please provide a valid API key
InvalidOrNonExistentMandate | 400 -- Bad Request | Mandate $mandateId cannot be used either because it is in an invalid state or it does not exist
PSPError | 400 -- Bad Request | Payment service provider error ($code): $message
LimitShouldBeStrictlyPositive | 400 -- Bad Request | Limit ($limit) should be strictly positive
MandateIsNotToBeSigned | 400 -- Bad Request | You cannot sign mandate $mandateRef because it is in status $status
MandateForbidden | 403 -- Forbidden | You cannot view mandate $mandateId because you do not manage one of its parties
MandateNotFound | 404 -- Not Found | Mandate $mandateId was not found
MandateWasNeverSigned | 400 -- Bad Request | You cannot download the document associated to mandate $mandateRef because it was never signed
MaximumLimitExceeded | 400 -- Bad Request | You cannot request $requestedLimit elements because it exceeds the maximum limit ($maximumLimit)
OffsetShouldBePositive | 400 -- Bad Request | Offset ($offset) should be positive
SCIAndUMRAlreadyUsed | 400 -- Bad Request | A mandate already exists with SCI (SEPA Creditor Identifier) $sci and UMR (Unique Mandate Reference) $umr
SinceShouldBeInThePast | 400 -- Bad Request | Since ($since) should be before now ($now)
UnexpectedDebtorForMandate | 400 -- Bad Request | Unexpected debtor id $unexpectedDebtorId for mandate $mandateRef that has debtor $expectedDebtorId
UntilShouldBeAfterSince | 400 -- Bad Request | Since ($since) should be before until ($until)
UserAlreadyExists | 400 -- Bad Request | User with email $email already exists
UserForbidden | 403 -- Forbidden | You cannot view user $userId because you do not manage it
UserHasNoActiveBankAccount | 400 -- Bad Request | User $name has no active bank account
UserHasNoSCI | 400 -- Bad Request | User $name cannot be a creditor because it does not have a SEPA Creditor Identifier (SCI)
UserNotFound | 404 -- Not Found | User $userId was not found
WebhookForbidden | 403 -- Forbidden | You cannot view webhook $webhookId because you do not manage its owner
WebhookNotFound | 404 -- Not Found | Webhook $webhookId was not found
