# Responsive sites

A site instance resource represents a single responsive Duda website. In the documentation below, responsive websites are referred to as multiscreen websites. They are the same thing. 

## Create site

Call the DudaAPI to create a new multiscreen site. This site will be created in the master account associated with the API key you are using. Creating a site will return a unique site name to reference this site resource.  Please note that sites created using this call will be set to Business+ status by default, but will not begin charging you until they are published. Please review the Payments and Publishing section for details.

### Method and path
`POST /sites/multiscreen/create`

### Arguments
> Full create site JSON representation. Only the template_id is required.

```json
{
	"template_id": 1000772,
	"url": "www.example.com",
	"default_domain_prefix": "sub-domain",
	"lang":"en",
	"site_data": {
		"site_domain": "www.example.com",
		"external_uid": "AD3421",
		"site_business_info": {
			"business_name": "Open Market",
			"phone_number": "4157775577",
			"email": "name@domain.com",
			"address": {
				"country": "US",
				"city": "San Francisco",
				"state": "CA",
				"street": "1 Market Street",
				"zip_code": "94111"
			},
			"opentable_info": [
				{
					"restaurant_id":"55123",
					"location":"Downtown",
					"country":"US"
				},
				{
					"restaurant_id":"55141",
					"location":"China Town",
					"country":"US"
				}
			]
		},
		"site_alternate_domains":{
			"domains":["www.domain1.com","www.domain1.net","www.domain2.com"],
			"is_redirect":true
		}
	}
}
```

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
template_id | Int | Required | Template ID of the template you want to base your new Responsive site on. You can see the API to get available templates [here](#get-templates).
url | String | optional | Have Duda import content into the new site based from an existing website. You may use an existing website address or Facebook business page. 
lang | String | optional | Set the language of the website. This controls the default copy (based on the template), widget outputs and other language specific text. See available [languages](#languages) above.
default_domain_prefix | String | optional | Set the default sub-domain for your site. This controls the <sub-domain-prefix>.dudaone.com address. 
site_data | Object | optional | JSON object containing additional site data. See below for the options and properties.
site_alternate_domain | Object | optional | Set alternate domains for the site. The alternate domain values will be redirected to the primary domain (set in the site_data object). 

The description for the site_data object is below:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
site_domain | String | optional | Set the domain of the site. This will be: www.example.com. This will be used when the site is published. This must be unique inside of the Duda platform. 
external_uid | string | optional | A flexible string you pass to Duda when creating the site, allows you to store your customer's unique value to reference later (45 char max). Note that Duda does not enforce uniqueness here.
business_site_info | Object | optional | Pass Duda detailed information about this business / website.

### site_business_info object
Provides structured data about a website or business. Duda will use this information to help populate the website with this data and fill the [content library](https://help.dudamobile.com/hc/en-us/articles/229790908-Manage-Content-Library) with data. All data fields are optional.

<aside class="notice">If you pass business_site_info during site create, this will trigger Duda's content import algorithm and we will attempt to import content from across the web based on the input data. This might result in a longer response time to create the website.</aside>

Property | Type | Description
---------- | ---------- | ----------
business_name | String |  The name of the business for the site being created
phone_number | String | The primary phone number for the business. 
address | Object | An object containing address details (city, state, zip, etc..)
opentable_info | Object | An object containing arrays of possible opentable IDs.

### Address object 
Providing an address helps the site creation process by giving Duda more information about the location of the business. For example, if you provide an address, we will automatically populate a maps element and potentially find other resources online to assist in site creation. 


Property | Type | Description
---------- | ---------- | ----------
Street | String | A street level line location (e.g. 123 Main Street)
State | String | The state of where the business is located
Country | String | The country of where the business is located
zip_code | String | The Zip Code or Postal Code of the business

### OpenTable object
Providing this will create an OpenTable button and help Duda find more information about the website online. This should be an array of JSON objects, as you can provide multiple restaurant_id's while creating the site.

Property | Type | Description
---------- | ---------- | ----------
restaurant_id | String | The unique OpenTable Restaurant ID. (e.g. 55123)
location | String | A description of the location (e.g. Downtown)
country | String | Two-digit country location (e.g. US, DE, JA)

### Site_alternate_domain object 
Allows you to define alternate or secondary domains associated with this site. By default, these domains will be 301 redirected to the primary domain. If you change the is_redirect value to false, the alternate domains will resolve the website instead of performing the redirect. Each alternate domain will need to have its DNS settings configured the same way as the primary domain, read here for how to set these up. 

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
domains | Array | optional | An array of strings that contain fully qualified domain names. Each domain will be set to redirect to the primary domain of the site.
is_redirect | bool | optional | Defaults to true. If set as true, all alternate domains will be set to 301 redirect to the primary domain.

>Create a website:

```shell
curl -X POST -i https://api.duda.co/api/sites/multiscreen/create \
	-u 'APIusername:APIpassword' \
	-H 'Content-Type: application/json' \
	-d '{ "template_id": "1000772"}'
```

> Expected return:

```json
{
	"site_name":"28e1182c"
}
```

### Return
Upon successful creation of a website, Duda will return a site_name value along with a `200 OK` HTTP response. The site_name is a unique string (usually between 8 and 32 characters) across all Duda websites and will be needed to later access other resources, such as updating the site, getting analytics or granting access. We recommend storing this site_name in your database for later reference.

## Get site
Get information about an already existing website. You will need the site_name value that was provided during the create website call. 

<aside class="warning">Note that only properties that have values will be returned.</aside>

`GET /sites/multiscreen/{site_name}`

### Required URI Parameters
site_name - URL Parameter. Originally returned when first creating a website.

> Response JSON from GET Site:

```json
{
  "account_name": "example@domain.com",
  "external_uid": "externalReference1",
  "site_domain": "customDomain1.com",
  "lang":"en"
  "site_business_info": {
    "business_name": "Example Business",
    "address": {
      "street": "1240 Main ST.",
      "city": "San Francisco",
      "state": "CA",
      "country": "United States",
      "zip_code": "94122"
    },
    "phone_number": "(415) 123-1234",
    "email": "example@domain.com",
    "opentable_info": []
  },
  "site_alternate_domains": {
    "domains": [
      "example1.com","example2.net"
    ],
    "is_redirect": true
  },
  "site_name": "6c56394c",
  "template_id": 20022,
  "site_default_domain": "exampleBusiness.multiscreensite.com",
  "preview_site_url": "http://websitebuilder.websitebusiness.com/preview/6c56394c",
  "last_published_date": "2016-11-15T22:55:53",
  "first_published_date": "2016-04-03T04:36:54",
  "last_reset_by": "ex@dudamobile.com",
  "force_https": false,
  "certificate_status": "COMPLETE",
  "publish_status": "PUBLISHED"
}
```

### Properties
Property | Type | Description
---------- | ---------- | ----------
template_id | String | The current template_id of the template in use on the site
site_domain | String | The primary domain currently assigned to the site
site_default_domain | String | Will return the default sub-domain of the site. This value will look like: <site_name>.dudaone.com or <site_name>.mutliscreensite.com (partners only). This will be returned even if the site is not published, the site is only accessible on this domain if it is published
site_name | String | The unique site name of the site, should be the same as the site_name returned during the creation of the site
account_name | String | The name (usually an email address) of the account that owns the site
preview_site_url | String | A direct URL to a the three-screen preview of the website in its current state while in design. This white labeled URL non-authenticated and can be accessed by anyone online
last_published_date | date time | The date of the most recent publication of the website. 
first_published_date | date time | The date of when this website was first published. Note that if you unpublished the website and republish it, the first_published_date will not change
publish_status | String | The current status of the website.  There are three possible values. (1) PUBLISHED: The website is live, published and being paid for. (2) UNPUBLISHED: The website was once published, but is now canceled and not available online. (3) NOT_YET_PUBLISHED: Means the site has never been published before
business_site_info | Object | [See above (in the create site section) for details of the business_info returned](#site_business_info-object). Duda will only return data that it has about the site
external_uid | String | Optional placeholder to be used to reference/link the created Site to some unique identifier origin from external system
force_https | Bool | If the website is redirecting all traffic to a secure (HTTPS) connection. If true, all traffic is sent to a secure connection
last_reset_by | String | The account name that has most recently reset the website.
cetrtificate_status | String | The status of SSL certificate generation. Has three possible values: "COMPLETE", "IN PROGRESS", or "FAILED"
site_alternate_domains | Object | [See the section above](#site_alternate_domain-object) to get the full reference for the site_alternate_domain object. 

```shell
curl -X GET https://api.duda.co/api/sites/multiscreen/57b6506a \
	-u 'APIpassword:APIusername' \
	-H 'Content-Type: application/json' 
```

## Get sites by External ID
This will return a list of all site ID's that have the external ID value provided when creating or updating the website. The external ID value is not a unique value in the Duda system, so multiple sites can have the same external ID value. 

`GET /sites/multiscreen/byexternalid/{external_uid}`

### Parameters

- external_uid - URL parameter

### Return

Duda will return an array of site_names for each website that has the matching external ID value. 

You should expect a 200 HTTP response code with the array of site names in the body of the response. 

> Example return: 

```json
[
  "6c56394c",
  "e714b0ba"
]
```

## Update site
You can use this call to update properties about a site that is already created. The most common use for this would be to update the site_domain of an already created site. Please note that only values listed here can be updated. Not all values in the get site info can be updated. 

### Method and Path
`POST /sites/multiscreen/update/{site_name}`

### Required Parameters

- Site Name - URI Parameter

> Example JSON to send: 

```json
{
	"site_domain":"example.com",
	"default_domain_prefix":"new-domain-prefix",
	"external_uid":"5123161232",
	"lang":"en",
	"site_alternate_domains":{
		"domains":[
			"alternatedomain1.com",
			"alternatedomain1.net"
		],
	"is_redirect":true
	}
}
```

### Parameters to send
Property | Type | Description
---------- | ---------- | ----------
site_domain | String ([FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)) | The domain of the site you want to use. Note that the domain value is unique across the entire Duda system. 
default_domain_prefix | String | Change the sub-domain prefix that you want to use. This must be a unique value and not used on other sites.
external_uid | String | Placeholder for external system identifier. (max 45 chars)
lang | String | Set the language of then website. This is used to change the output of default strings in widgets, cookie notifications, etc..
site_alternate_domains | Object | Set alternate domains for the site. The alternate domain values will be redirected to the primary domain. 
force_https | bool | If true, all website traffic will be redirected to a secure (HTTPS) enabled site. Only can be set if an SSL certificate is successfully generated. Defaults to true. 

> Example CURL to Update Site:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/update/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"site_domain":"www.uniquedomain.com"}'
```

### Return
A `204 No Content` HTTP code will be returned upon successful call. 

## Delete site
This will immediately and permanently delete the site and cancel any subscription associated with the site. Please note that after deleting a site there is no way to bring the site back. Deleting a site will also cancel any active subscription/payment associated with the site. 

### Method and path

`DELETE /sites/multiscreen/{site_name}`

### Parameters
- Site Name: URI Parameter

> CURL Example to delete a site:

```shell
curl -X DELETE -k 'https://api.duda.co/api/sites/multiscreen/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
Duda will return a `204 No Content` HTTP code for successful calls. 

## Publish site
Takes the development version of the site and copies it to a live state. Publishing a site for the first time will also charge your account and create a new subscription unless you have included the test parameter. You can publish a site at any time, which will overwrite the current published version of the site with the current development version.

### Method and path

`POST /sites/multiscreen/publish/{site_name}`

### Parameters

- Site Name - URI Parameter

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/publish/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
Duda will return a `204 No Content` HTTP code for successful calls. 

## Unpublish site
Removes the site from the production environment. This is good for disabling the site or taking it down temporarily so no one can access it. This will not delete the website. The website will still exist and can be published later. 

### Method and path
`POST /sites/multiscreen/unpublish/{site_name}`

### Parameters

- Site Name - URI Parameter

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/publish/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
Duda will return a `204 No Content` HTTP code for successful calls. 

## Duplicate site

Create a duplicate of a single multiscreen site. The new site will not be published and/or will lose the site_domain value provided to the previous site. Any customizations done on the site, within the website builder, will be copied to the new version of the site.

### Method and path

`POST /sites/multiscreen/duplicate/{site_name}`

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/duplicate/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	\ 
	-d '{"new_default_domain_prefix":"bobspizza"}'
```

### Parameters 
- Site_Name: URI Parameter

The following parameters can be sent in the body for the new website that will be created. They are all optional

Property | Type | Description
---------- | ---------- | ----------
new_default_domain_prefix | String | If the data set is included, it will set the default prefix domain of the newly duplicated site. For example, if you send bobspizza, the default domain will be: bobspizza.dudaone.com (or bobspizza.multiscreensite.com for partners). This must be a unique value across the system.
new_external_uid | String | Specify a new external_uid (external user id) to the site when duplicating it. 

### Return
Duda will respond with a `200 OK` HTTP status and return a new site_name.

```json
{
	"site_name":"a6119858"
}
```
## Reset site

Reset and choose a new template for this site. This allows you to keep an existing site_name -- but perform a reset on the site via API. 

### Method and Path

`POST /sites/multiscreen/reset/{site_name}`

### Parameters

- You must pass the site_name value as part of the POST request

> Example of additional input:

```json
{
  "template_id": "20001",
  "site_data": {
    "removeBizInfos": true
  }
}
```

### Input Arguments

As part of the body request, you can pass any data that is available in the standard [create website](#create-site) API call. There is one additional option you can pass to Duda as part of the create site JSON:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
removeBizInfos | Bool | Optional | If passed as true, resetting the site will remove all business information that exists within the content library. By default, Duda keeps business information in the content library when resetting the website. 



### Return 

You can expect a `204 No Content` response code for a successful reset call.


## Get multiple sites

Get site details for many sites at once. You can send an array of site names you can retrieve information about multiple sites at once. This is much quicker than calling the retrieve site multiple times. A maximum of 50 sites can be retrieved at once.

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/get-many' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	\
	-d '[{"site_name":"fb8b8ced"},{"site_name":"76ed3209"}]'
```

### Method and path

`POST /sites/multiscreen/get-many`

### Parameters 
An array of site name strings should be sent in the body of the request. 

### Return

> JSON Return Example: 

```json
{
  "sites": [
    {
      "account_name": "example@domain.com",
      "site_domain": "example12.com",
      "site_name": "76ed3209",
      "template_id": 20022,
      "site_default_domain": "russ12.dudaone.com",
      "preview_site_url": "http://websitebuilder.company.com/preview/76ed3209",
      "last_published_date": "2015-10-23T17:52:14",
      "first_published_date": "2014-06-24T17:13:42",
      "force_https": false,
      "publish_status": "PUBLISHED"
    },
    {
      "account_name": "example@domain.com",
      "site_domain": "example3.com",
      "site_business_info": {
        "business_name": "Pacific Coast Well Drilling",
        "address": {
          "street": "75 N Main St",
          "city": "Templeton",
          "state": "CA",
          "country": "United States",
          "zip_code": "93465-5101"
        },
        "phone_number": "(805) 434-3121",
        "email": "Example@domain.com",
        "opentable_info": []
      },
      "site_name": "fb8b8ced",
      "template_id": 20012,
      "site_default_domain": "russ257.multiscreensite.com",
      "preview_site_url": "http://websitebuilder.company.com/preview/fb8b8ced",
      "last_published_date": "2016-07-05T19:17:17",
      "first_published_date": "2015-08-10T01:02:16",
      "force_https": true,
      "certificate_status": "COMPLETE",
      "publish_status": "PUBLISHED"
    }
  ]
}
```

Duda will return a sites object with an array of sites. The details of the site values returned will be the exact same as the [get site example above](#get-site), please see there for details about values returned.

You can expect a `200 OK` response code. 
