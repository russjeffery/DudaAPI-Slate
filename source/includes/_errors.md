# Errors

> Attempt to get site info for a site that does not exist:

```shell
curl -X GET -i 'https://api.dudamobile.com/api/sites/multiscreen/badName' \
    -H 'Authorization: ZXhhbXBsZVVzZXI6YmUkdHBAc3M=' \ 
    -H 'Content-Type: application/json'
```
> Duda responds with a 400 HTTP code and the following body:

```json
{
  "error_code": "ResourceNotExist",
  "message": "Site with alias 'badName' doesn't exist"
}
```

All API methods will respond with one of the following HTTP response codes:


HTTP Code | Description
---------- | -------
200 | OK -- Your request was successful and the response body contains a JSON representation
204 | OK - No response. Your request was successful and Duda had no data to return to you
302 | Found - A common redirect response; you can GET the representation at the URI in the Location response header
400 | Bad Request - The data given in the request failed validation. Inspect the response body for details (see Error data structure for details).
401 | Unauthorized - Duda failed to authenticate your request
404 | Not Found -- The specified method / path could not be found
405 | Method Not Allowed -- You tried to access the method
500 | This usually means there was a problem on Duda's end. Double check to make sure your data sent is correct then reach out to Duda for help.

When Duda detects an error in your request, we will respond with a 400 (Bad Request) HTTP response and return a JSON error object:

Property | Type | Description
---------- | ---------- | ----------
error_code | String | Code of the error: ResourceNotExist, InvalidInput, or InternalError
message | String | A detailed error message of what went wrong