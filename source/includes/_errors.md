# Errors

The Bulk SMS API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request does not conform the expected format (Invalid data)
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- You do not have appropriate permissions to access API
404 | Not Found -- The URL specified does not exist.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
