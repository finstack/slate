# HTTP Error Codes

The Finstack API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- There was an error while processing the request payload (malformed JSON, for instance)
401 | Unauthorized -- The request is not authenticated, or invalid API key
403 | Forbidden -- The request is successfully authenticated (see 401), but the action requires a different type of key
404 | Not Found -- There is no resource behind the URI
405 | Method Not Allowed -- You tried to access a resource with an invalid method (GET instead of POST for example)
406 | Not Acceptable -- You requested a format that isn't JSON
429 | Too Many Requests -- You have sent too many requests in a given amount of time ("rate limiting")
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
