# Errors

pCloudy uses conventional HTTP response codes to indicate the success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that failed given the information provided(e.g., a required parameter was omitted, etc.), and codes in the 5xx range indicate an error with pCloudy’s servers(these are rare).


Error Code | Meaning
---------- | -------
200 | Ok -- Everything worked as expected.
400 | Bad request -- The request was unacceptable.
401 | Unauthorized -- Your email or API key is wrong.
402 | Request failed -- The parameters were valid but request failed.
404 | Not found -- The requested resource doesn’t exits.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
