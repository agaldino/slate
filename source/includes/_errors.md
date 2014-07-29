# Errors

The SmartWallet API uses the following error codes:


Error Code | Meaning
---------- | -------
042 | Answer to Life, Universe and Everything
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your SmartWalletKey is wrong
403 | Forbidden -- You don't have access to that.
404 | Not Found -- The object, Transaction or Account could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
418 | I'm a teapot 
429 | Too Many Requests -- You're going crazy here man! Slown down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.