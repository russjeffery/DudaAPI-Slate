# Introduction

Welcome to the Duda API REST Documentation. Here you will find APIs for Multiscreen (Responsive), Analytics and account management. Duda's REST API is designed to be a management API, which means you can integrate Duda's White Labeled resources (Sites, Accounts, etc..) into your own workflows or services. It is not currently designed to manage individual edits to websites.

The API uses standard RESTful principles to manage resources:

* POST verb/method will create or update a resource
* GET verb/method will read an existing resource
* DELETE verb/method will delete an existing resource

## Authentication

```shell
curl -X GET -i 'https://api.dudamobile.com/api/sites/multiscreen/templates' \
	-H 'Authorization: ZXhhbXBsZVVzZXI6YmUkdHBAc3M=' \ 
	-H 'Content-Type: application/json'
```
> Instead of explicitly setting the Authorization header, you can use the -u 'APIUser:APIPass' command with cURL instead.

The Duda API uses <a href="https://en.wikipedia.org/wiki/Basic_access_authentication" target="_blank">basic HTTP authentication</a> to authenticate requests and access resources within your account. As part of getting access to the API, you will get a user name and password combination that is specific to your account. This information should be passed as part of every API request you make to the platform. It is also required that you send this in a preemptive fashion. 

To create the HTTP Authorization Header, you should combine your username and password into one string, separated by a colon and then Base64 encode that string. So for example, 'exampleUser:be$tp@ss' combination would result in the header:

`Authorization: ZXhhbXBsZVVzZXI6YmUkdHBAc3M=`

<aside class="notice">
Make sure to set the authorization header with your unique API username and API password on each request.
</aside>

## Security 

The Duda REST API is available over HTTPS only to ensure data privacy. Unencrypted HTTP is not supported. As of the current version, the Duda REST API is secured with HTTP Basic Authentication only.

Duda recommends requiring a TLS connection instead of SSL. The Duda API does not accept SSLv3 connections, due to the [POODLE venerability](http://en.wikipedia.org/wiki/POODLE).  

## Access

To access the Duda API, you need to request access through your websites dashboard. This can be found under the 'Account tab' at the top of the dashboard navigation. Once you request access, Duda will need to approve the request, which takes upto 48 hours. You will only receive your password via email. Please keep the API Username and Password safe, as anyone with this information can access your account. 

The Duda API is free for anyone to access and use. You should take note of the payments section as we currently do not offer free ad-supported sites created via the API.

## Endpoints

Duda's primary endpoint for all production work is: 

`https://api.dudamobile.com/api`

The documentation here assumes that you will combine the endpoint with the path of the API method. There are some cases where you might have a different endpoint than the one above, such as working with Duda's sandbox environment or a custom setup that Duda provides, although this is rare. 

## Data schema

All data passed to and from the Duda API will be in JSON representation. We currently only support this format of data transfer.

## Dates

There are some Duda APIs that return date time values. Duda formates these in standard universal full date/time pattern in the UTC timezone. The format is: yyyy-mm-ddThh:mm:ss. So for example: 2016-11-15T22:55:53 would be November 11th, 2016 at 10:55:53 PM UTC.

When sending dates to Duda, you can use the full universal format above, or just the YYYY-MM-DD format. If you omit the time, Duda will default that time to the beginning of the day. 

## Langauges

There are some parts of the API where Duda returns real world content to display to users. As part of this, we are able to deliver localized content. Below you will see a full list of languages that the Duda platform (UI) is localized for along with their corresponding language codes. 


Language | Language Code
--------- | -------
English | en
Spanish LATAM | es
German | de
Portuguese | pt
Turkish | tr
English UK | en_gb
Italian | it
Dutch | nl
French | fr
Indonesian | id
Polish | pl

## Errors

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
405 | Method Not Allowed -- The GET/POST/DELETE/PUT Method you input is not supported for this path
500 | This usually means there was a problem on Duda's end. Double check to make sure your data sent is correct then reach out to Duda for help.

When Duda detects an error in your request, we will respond with a 400 (Bad Request) HTTP response and return a JSON error object:

Property | Type | Description
---------- | ---------- | ----------
error_code | String | Code of the error: ResourceNotExist, InvalidInput, or InternalError
message | String | A detailed error message of what went wrong