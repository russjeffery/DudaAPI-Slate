## Get recently published sites

Get a list of recently published websites in your account, for a specific amount of days. This returns results of all sites that have been published recently, either for an ongoing website update or for the first time. This can be used in conjunction with the get site to see the status of the website for billing reasons. You can call this API to get recently published websites, then get the actual status of the website by calling the get site to see if it is published, unpublished or has never been published.

If you do not pass a lastDays value, then Duda will return results for the last 7 days by default.

### Method and path

`GET /sites/multiscreen/published`

> JSON Response:

```json
[ 
"a75b4ddc", 
"d2f8eded" 
]
```

### URI Parameters:
- lastDays: The number of days in which you would like get sites that have been published. 

### Response

Duda will return a `200 OK` response code along with an array of strings that are Duda Site_name values. 

>CURL Example:

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/published?lastDays=30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```
## Get recently unpublished sites

Get a list of recently unpublished websites in your account, for a specific amount of days. This returns results of all sites that have been unpublished recently, which usually means the website was canceled. This can be used in conjunction with the get site to see the status of the website for billing reasons. You can call this API to get recently unpublished websites, then get the actual status of the website by calling the get site to see if it is published or unpublished.

If you do not pass a lastDays value, then we will return results for the last 7 days by default.

> JSON Response:

```json
[ 
"a75b4ddc", 
"d2f8eded" 
]
```

### Method and path

`GET /sites/multiscreen/unpublished`

### URI Parameters:
- lastDays: The number of days in which you would like get sites that have been published. 

>CURL Example:

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/unpublished?lastDays=30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Response

Duda will return a `200 OK` response code along with an array of strings that are Duda Site_name values. 

## Get sites created between

Will return an array of site names created during the given time period. If no from parameter is specified, sites created in the past 7 days will be returned. Will return an array of site names. Please see the section about [handling dates, above.](#dates)

### Method and path
`GET /sites/multiscreen/created?from=2016-03-01&to=2016-03-31`

> JSON Return:

```json
[
   "c27cdebf",
   "1aadcb7c",
   "0c2b5c42",
   "83729462",
   "287185af",
   "ff82ddc4",
   "a6119858",
   "57b6506a",
   "5876c248",
   "6f45aed8"
]
```

### URL Query Parameters
Query String | Type | Required | Description
---------- | ---------- | ---------- | ----------
from | Date | Optional | Start date to query sites for. If not supllied, defaults to 7 days ago, from the current time. 
to | Date | Optional | End date of when to search for. If not supplied, will default to today.

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/created?from=2016-03-01&to=2016-03-09' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
You can expect a `200 OK` HTTP code along with an array of site names to be returned for the specified time frame. 

## Get contact form data

Get all the contact form submissions that happened on this site. This call will return an array of each contact form submission.

> Example JSON Response: 

```json
[
   {
      "form_title":"Contact Us",
      "message":{
         "Name":"test3",
         "Phone":"123-123-1234",
         "Dropdown":"Option 2",
         "Email":"test@test.com",
         "Message":"test message",
         "Radio button":"Option 2",
         "Date":"02/05/2014",
         "Check box":"Option 2"
      },
      "date":"2014-05-08T23:12:06"
   },
   {
      "form_title":"Contact Us",
      "message":{
         "Name":"test",
         "Phone":"abcc",
         "Email":"test@test.com",
         "Message":"123"
      },
      "date":"2014-05-08T22:34:50"
   }
]
```

### Method and path

`GET /sites/multiscreen/get-forms/{site_name}`

### Required Parameters:
- site_name

### URL Parameters

You can add on additional *to* or *from* URL parameters onto the full URL to decide which date range you would like to get contact form details from. These should be in [standard date format](#dates).

<aside class="notice">Duda will return all contact form messages that have been submitted to the site. Each form on the site has a *form title*, controlled in the Duda Editor, which will allow you to tell which contact form on the site this is from.</aside>

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/get-forms/b4ra2g?from=2017-01-01&to=2017-01-30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```
