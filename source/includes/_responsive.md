# Responsive sites

A site instance resource represents a single Duda site. There are two main types of Duda sites: mobile and multiscreen (responsive). Below, you will see the API resources available for the multiscreen sites only.

## Create site

Call the DudaAPI to create a new multiscreen site. This site will be created in the master account associated with the API key you are using. Creating a site will return a unique site name to reference this site resource.  Please note that sites created using this call will be set to Business+ status by default, but will not begin charging you until they are published. Please review the Payments and Publishing section for details.

### Method and path
`POST /sites/multiscreen/create`

### Arguments
> Full create site JSON representation. Only the template_id is required.

```json
{
	"template_id": 20012,
	"url": "www.example.com",
	"default_domain_prefix": "sub-domain",
	"site_data": {
		"site_domain": "www.example.com",
		"external_uid": "AD3421",
		"site_business_info": {
			"business_name": "Open Market",
			"phone_number": "4157775577",
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
		}
	},
	"site_alternate_domain":{
		"domains":["www.domain1.com","www.domain1.net","www.domain2.com"],
		"is_redirect":true
	}
}
```

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
template_id | Int | Required | Template ID of the template you want to base your new Responsive site on. You can see the API to get available templates [here](#get-templates).
url | String | optional | Have Duda import content into the new site based from an existing website. You may use an existing website address or Facebook business page. 
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
curl -X POST -i https://api.dudamobile.com/api/sites/multiscreen/create \
	-u 'APIusername:APIpassword' \
	-H 'Content-Type: application/json' \
	-d '{ "template_id": "20012"}'
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
curl -X GET https://api.dudamobile.com/api/sites/multiscreen/57b6506a \
	-u 'APIpassword:APIusername' \
	-H 'Content-Type: application/json' 
```

## Get sites by external id
This will return a list of all site ID's that have the external ID value provided when creating or updating the website. The external ID value is not a unique value in the Duda system -- so multiple sites can have the same external ID value. 

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
site_alternate_domains | Object | Set alternate domains for the site. The alternate domain values will be redirected to the primary domain. 
force_https | bool | If true, all website traffic will be redirected to a secure (HTTPS) enabled site. Only can be set if an SSL certificate is successfully generated. Defaults to true. 

> Example CURL to Update Site:

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/update/57b6506a' \
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
curl -X DELETE -k 'https://api.dudamobile.com/api/sites/multiscreen/57b6506a' \
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
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/publish/57b6506a' \
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
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/publish/57b6506a' \
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
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/duplicate/57b6506a' \
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
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/created?from=2016-03-01&to=2016-03-09' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
You can expect a `200 OK` HTTP code along with an array of site names to be returned for the specified time frame. 

## Get multiple sites

Get site details for many sites at once. You can send an array of site names you can retrieve information about multiple sites at once. This is much quicker than calling the retrieve site multiple times. A maximum of 50 sites can be retrieved at once.

> CURL Example:

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/get-many' \
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

## Get all templates

Get an array of objects for all templates available to your account. Each template has a set of data associated with it, including the template_id, which is required to create new sites from. Both Duda default templates and custom templates that you create will be returned by this API call. 

### Method and path

`GET /sites/multiscreen/templates`

> Example of returned template data:

```json
[
	{
		"template_name": "Yellow Brick Road",
		"preview_url": "http://example.mobilewebsiteserver.c...heme-20004-263",
		"thumbnail_url": "https://dd-cdn.multiscreensite.com/t...brick-road.jpg",
		"template_id": 20004,
		"template_properties": {
			"can_build_from_url": true
		}
	},
	{
		"template_name": "Popsicle",
		"preview_url": "http://example.mobilewebsiteserver.c...heme-20002-263",
		"thumbnail_url": "https://dd-cdn.multiscreensite.com/t...w/popsicle.jpg",
		"template_id": 20002,
		"template_properties": {
			"can_build_from_url": true
		}
	},
	{
		"template_name": "Medical",
		"preview_url": "http://example.mobilewebsiteserver.c...heme-20021-263",
		"thumbnail_url": "https://dd-cdn.multiscreensite.com/t...ew/medical.jpg",
		"template_id": 20021,
		"template_properties": {
			"can_build_from_url": true
		}
	},
	{
		"template_name": "Custom2 Template",
		"preview_url": "http://example.mobilewebsiteserver.c...eview/f8b71a68",
		"thumbnail_url": "https://dp-cdn.multiscreensite.com/t...nd=[B@676f0be9",
		"template_id": 1000410,
		"template_properties": {
			"can_build_from_url": false
		}
	}
]
```

### Optional URI Parameter

You can add a `?lang=en` URL parameter onto the template API path call to get the templates for specific languages. You can see the languages available via the API here.

### Properties

Property | Type | Description
---------- | ---------- | ----------
template_name | String | The name of the template.
preview_url | URL String | A direct URL to preview the template. This URL is publically available and does not require authentication.
thumbnail_url | URL String | A direct URL to a JPG image displaying the template. This would be good to use to show in a template selection page. 
template_id | Int | A unique number associated with this template. This is used to create the template from. 
template_properties | Object | Contains proprieties of the templates. Currently this only contains can_build_from_url
can_build_from_url | Bool | Will be either true or false. If it is false, this means that the template is a custom template which cannot pull content from an external URL while creating the site. 

> CURL Example to Get All Templates:

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/templates' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

You can expect a `200 OK` response along with the all the template data (in an array of objects) shown here.

## Get template info

Get the name, preview URL, thumbnail_url, and template ID of a single template. You can use this to get particular images or preview URLs of templates to display to your users. This will allow them to see the variety of templates to choose from. If you want to see the full list of template ID's available, please use the [get all templates call](#get-all-templates).

> JSON Data Returned:

```json
{
	"template_name": "Italiano",
	"preview_url": "http://example.mobilewebsiteserver.com/preview/dm-theme-20012-263",
	"thumbnail_url": "https://dd-cdn.multiscreensite.com/themes-panel/preview/italiano.jpg",
	"template_id": 20012,
	"template_properties": {
		"can_build_from_url": true
	}
}
```

### Method and path

`GET /sites/multiscreen/templates/{template_id}`

### Parameters: 

- template_id: The specific ID associated with this template. Originally returned via the get all templates API call. 

### Optional URI Parameter

You can add a `?lang=en` URL parameter onto the template API path call to get the templates for specific languages. You can see the languages available via the API here

> CURL Example:

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/templates/20012' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

You can expect a `200 OK` response along with an JSON object that describes the given template. 

## Create template from site

Take an existing website and turn it into a template. This template can then be used to build new sites from. The template will be returned as part of the GET TEMPLATES api call and be available to select from while creating a new site (under the My Templates section while creating a new multiscreen site).

### Method and path

`POST /sites/multiscreen/template/fromsite`

> Example JSON to send:

```json
{
    "site_name": "66cbb9b7",
    "new_template_name": "Example Template"
}
```

### Parameters

Send the following parameters when creating a new template: 

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
site_name | String | Required | A valid site name of an existing website. Originally returned after creating the website. 
new_template_name | String | Required | A name for the template you are creating. 

> Example to create template: 

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/template/fromsite' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \ 
	-d '{"site_name": "66cbb9b7","new_template_name": "Example Template"}'
```
### Response:
You can expect a `200 OK` response code alongside a template object that describes the new template.

> JSON Return:

```json
{
    "template_name": "Example Template",
    "preview_url": "http://example.mobilewebsiteserver.c...eview/365dd849",
    "thumbnail_url": "https://dp-cdn.multiscreensite.com/t...nd=[B@311b938d",
    "template_id": 1000408,
    "template_properties": {
        "can_build_from_url": false
    }
}
```

<aside class="notice">Often when first creating a template, the thumbnail_url returned is a generic placeholder image. Duda will automatically generate a thumbnail image of the template as an asynchronous process that is often available a while after the initial template creation.</aside>

## Create template from template

Take an existing template and turn it into a new template. This template can then be used to build new sites from. The template will be returned as part of the GET TEMPLATES api call and be available to select from while creating a new site (under the My Templates section while creating a new multiscreen site).

### Method and path

`POST /sites/multiscreen/template/fromtemplate`

> Example JSON to send:

```json
{
    "template_id": 1000410,
    "new_template_name": "Example Template"
}
```

### Parameters

Send the following parameters when creating a new template: 

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
template_id | Int | Required | An ID of an already existing website template.
new_template_name | String | Required | A name for the template you are creating. 

> Example to create template: 

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/template/fromtemplate' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \ 
	-d '{"template_id":20012,"new_template_name": "Example Template"}'
```
### Response:
You can expect a `200 OK` response code alongside a template object that describes the new template.

> JSON Return:

```json
{
    "template_name": "Example Template",
    "preview_url": "http://example.mobilewebsiteserver.c...eview/365dd849",
    "thumbnail_url": "https://dp-cdn.multiscreensite.com/t...nd=[B@311b938d",
    "template_id": 1000410,
    "template_properties": {
        "can_build_from_url": false
    }
}
```

<aside class="notice">Often when first creating a template, the thumbnail_url returned is a generic placeholder image. Duda will automatically generate a thumbnail image of the template as an asynchronous process that is often available a while after the initial template creation.</aside>

## Delete custom template

Delete a previously created custom template. This will not delete any sites that have been created from the template, but it will remove it from your list of available templates.

### Method and path

`DELETE /sites/multiscreen/templates/{template_id}`

### URI Parameters:
- template_id: The unique ID of the template; 

```shell
curl -X DELETE -k 'https://api.dudamobile.com/api/sites/multiscreen/template/100041' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Response

Upon successful deletion, a `204 No Content` HTTP code will be returned. 

## Update custom template 

Update the name of an existing custom template.

> Example JSON to send:

```json
{
    "new_name": "New template name"
}
```

### Method and path

`POST /sites/multiscreen/templates/{template_id}`

### URI Parameters

- new_name: A string that contains a new name you want the template.

```shell 
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/template/100041' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"new_name":"New template name"}'
```
### Return 

Upon successful update a `204 No Content` HTTP response code will be returned.

## Get pages

Get all pages that exist within the website. Returns an array of objects that describes each page of the site. 

### Method and path

`GET /api/sites/multiscreen/site/{site_name}/pages`

> CURL Example:

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/b4ra2g/pages' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

> JSON Representation:

```json
[
  {
    "page_title": "ABOUT",
    "page_path": "about/abab",
    "page_name": "YWJvdXQvYWJhYg"
  },
  {
    "page_title": "SERVICES",
    "page_path": "services",
    "page_name": "c2VydmljZXM"
  },
  {
    "page_title": "HOME",
    "page_path": "home",
    "page_name": "aG9tZQ"
  },
  {
    "page_title": "CONTACT",
    "page_path": "contact",
    "page_name": "Y29udGFjdA"
  }
]
```
### Properties required
- site_name - Internal Site Reference

### Properties returned

Property | Type | Description
---------- | ---------- | ----------
page_name | String | The internal unique name of the specific page. This value is used to update or delete the page with other API calls. Note that this is updated when the path of the page is changed.
page_title | String | The title of the page in the navigation of the site and also in the pages menu. 
page_path | String | The path / URL to this page on the website. 

### Return

You can expect a `200 OK` response code with an array of JSON objects describing each page. 

## Update page

Update an existing page on the website. You can update either the page_title or the page_path of a specific page. You need to pass the page_name to update it, which is returned via the [GET Pages](#get-pages) API Call.

### Method and path

`POST /api/sites/multiscreen/site/{site_name}/pages/{page_name}/update`

### Required Properties
- site_name - The name of the site, returned when it was originally created
- page_name - The name of the page, returned from the get page(s) API call

> Example:

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/b4ra2g/pages/Y29udGFjdA/update' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \ 
	-d '{ "page_path":"contact/1","page_title":"contact 1"}'
```
### Return

You can expect a `204 No Content` HTTP response for a successful call.

<aside class="warning">You cannot update the page_path of the home page.</aside>

## Get page

Get the details of an individual page of the site. Returns a JSON object containing the page_title, page_path and unique page_name.

>Example

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/b4ra2g/pages/YWJvdXQvYWJhYg' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Method and path

`GET /api/sites/multiscreen/site/{site_name}/pages/{page_name}`

> JSON Return

```json
{
  "page_title": "ABOUT",
  "page_path": "about/abab",
  "page_name": "YWJvdXQvYWJhYg"
}
```

### Required URI inputs
- site_name - Name of the site, originally returned when the site was created.
- page_name - Name of the page, originally returned via the [Get Sites API call](#get-sites).

### Return
You can expect a `200 OK` response HTTP code for a successful call. 

## Delete page

Delete a page of your website. This cannot be undone.

> Example

```shell
curl -X DELETE -k 'https://api.dudamobile.com/api/sites/b4ra2g/pages/YWJvdXQvYWJhYg' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Method and path

`DELETE /api/sites/multiscreen/site/{site_name}/pages/{page_name}/delete`

### Required URI Inputs
- site_name - Name of the site, originally returned when creating the website. 
- page_name - Name of the page, originally returned when getting the pages of the site. 

### Return

You can expect a `204 No Content` HTTP code upon success. 

## Inject content

> Basic structure of inject content data

```json
[
  {
    "type": "INNERHTML",
    "key": "dataInjectValueToSearchFor",
    "value": "valueToReplaceContentWith"
  },
  {
    "type": "DOMATTR",
    "key": "dataInjectValueToSearchFor",
    "value": "attributeValueToAddToRefs",
    "refs": [
      "placeholderKeyToAddOrRepace1","placeholderKeyToAddOrRepace2"
    ]
  },
  {
    "type": "CSS",
    "key": "dataInjectValueToSearchFor",
    "value": "cssValueToAddToBlock:rgba(0,0,0,1)",
    "refs": [
      "cssPropertiesToAddOrReplace1","background-color","background"
    ]
  }
]
```

Content injection allows you to change a website via API (text, images, CSS, etc..) direclty on an existing website or template. You can update CSS values, InnerHTML of an elemnt or an attribute on an element. For the InnerHTML and Attr types, you must have the `data-inject=value` set on the element. For the CSS type, you must have a `data-inject:value` CSS property set within a declaration block. 

This feature primarily works by sending the `type` value to alter the markup or styling in multiple ways. Should pass Duda an array of JSON objects. For a full example of content injection, [please see this guide](#NEEDS-LINK).


### Method and path

`POST /api/sites/multiscreen/inject-content/{siteName}`

### Types of injection

Below you will see the three types of injection available: `CSS`, `DOMATTR`, and `INNERHTML` along with how Duda expects each one to be sent.

> Example

```shell
# If you have this HTML & CSS block within the website:
#
# <a class="u_1454453128 dmNewParagraph email-css" data-inject="email" id="1454453128" href="mailto:oldEmail@domain.com">oldEmail@domain.com</a>
#
# .email-css {
#	data-inject: email-css;
#	color: #474747;
# }
#
# And run this: 
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/inject-content/b4ra2g' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '[
			{
				"type": "INNERHTML",
				"key": "email",
				"value": "newEmail@domain.com"
			},
			{
				"type": "DOMATTR",
				"key": "email",
				"value": "mailto:newEmail@domain.com",
				"refs": [
					"href"
				]
			},
			{
				"type": "CSS",
				"key": "email-css",
				"value": "#000000",
				"refs": [
					"color"
				]
			}
		]'
# The code will be updated to:
# <a class="u_1454453128 dmNewParagraph email-css" data-inject="email" id="1454453128" href="mailto:newEmail@domain.com">newEmail@domain.com</a>
#
# .email-css {
#	data-inject: email-css;
#	color: #000000;
# }
```

### INNERHTML 

Property | Type | Description
---------- | ---------- | ----------
type | String | **INNERHTML**: Replaces all content (HTML, text, etc..) within this element. 
key | String | The value of the `data-inject` attribute on the element. For example, if you have `data-inject=email` you should place `email` as the value. Duda will then search through every page of the site and replace the innerHTML of all elements that contain the `data-inject=email` attribute.
value | String | The content you want to be placed within the element. Can be static text, HTML, etc.

### DOMATTR
Property | Type | Description
---------- | ---------- | ----------
type | String | **DOMATTR**: Allows you to add or update the attributes of the element. 
key | String | The value of the `data-inject` attribute on the element. For example, if you have `data-inject=email` you should place `email` as the value. Duda will then search through every page of the site and add or replace the DOM attributes of all elements that contain the `data-inject=email` attribute.
value | String | The value of the attribute you are wanting to add. For example, if you want to change the `src` of an image, you would place the direct URL of the image as the value. 
refs | Array of Strings | The keys of the attributes you want to add or replace. For example, if you want to alter the `src` of an image, you would place `src` as a string here. 

### CSS
Property | Type | Description
---------- | ---------- | ----------
type | String | **CSS**: Allows you to add or update CSS values within the block that the CSS value `data-inject:name` is present.
key | String | The value of the `data-inject` attribute within the CSS block. For example, if you have `data-inject:email-css` you should place `email-css` as the value here. Duda will then search through every CSS file of the site and replace the values of the reference strings you pass.
value | String | The CSS value you want to update. This could be `none` for the display property, a URL for background-image or a color code (HEX or RGB) for color values, for example. 
refs | Array of Strings | The property of the CSS you want to update. This will find the property you pass here and update with the value above.


### Return 

Duda will return a `204 No Content` HTTP response code.

<aside class="notice">The inject content API only updates the development version of the website. If you'd like to see these changes on the live website, make sure that you publish the website after making these updates.</aside>

## Get Inject Content Values

Search the website for all references of the data-inject value, either in the HTML or CSS of the website. This returns to you the existing properties that can be used for data injection later.

This can also be used to get the actual content of a specific element within the website. 

> Example 1 of one button within the website that contains a data-inject="read-more-btn" attribute assigned to it:

```shell
curl -u 'APIusername:APIpassword' \  
-X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/inject-content/b4ra2g' \
-H 'Content-Type: application/json'
```

> Response 1

```json
[
  {
    "key": "read-more-btn",
    "type": "DOMATTR",
    "ref": "class",
    "value": "u_1330691906 align-center dmButtonLink dmWidget dmWwr default dmOnlyButton dmDefaultGradient"
  },
  {
    "key": "read-more-btn",
    "type": "DOMATTR",
    "ref": "href",
    "value": "/about"
  },
  {
    "key": "read-more-btn",
    "type": "DOMATTR",
    "ref": "dmle_widget",
    "value": "dudaButtonLinkId"
  },
  {
    "key": "read-more-btn",
    "type": "DOMATTR",
    "ref": "id",
    "value": "1330691906"
  },
  {
    "key": "read-more-btn",
    "type": "DOMATTR",
    "ref": "data-inject",
    "value": "read-more-btn"
  },
  {
    "key": "read-more-btn",
    "type": "INNERHTML",
    "value": "<span class=\"iconBg\" id=\"1206527425\" duda_id=\"1206527425\"> <span class=\"icon hasFontIcon icon-star\" id=\"1686467387\" duda_id=\"1686467387\"> </span></span><span class=\"text\" id=\"1307805474\" localization_key=\"templates.custom.197\" duda_id=\"1307805474\">      Read More     </span>"
  }
]
```

> Example 2, filtering for only the INNERHTML:

```shell
curl -u 'APIusername:APIpassword' \  
-X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/inject-content/b4ra2g?type=INNERHTML' \
-H 'Content-Type: application/json'
```

> Example 2 response:

```json
[
  {
    "key": "read-more-btn",
    "type": "INNERHTML",
    "value": "<span class=\"iconBg\" id=\"1206527425\" duda_id=\"1206527425\"> <span class=\"icon hasFontIcon icon-star\" id=\"1686467387\" duda_id=\"1686467387\"> </span></span><span class=\"text\" id=\"1307805474\" localization_key=\"templates.custom.197\" duda_id=\"1307805474\">Read More</span>"
  }
]
```

### Method and path
`GET /api/sites/multiscreen/inject-content/{site_name}`

### Optional query parameters

You can append the following query parameters to filter the results of what will be returned:

Property | Type | Description
---------- | ---------- | ----------
key | String | Search for a specific element that contains the data-inject attribute or CSS block that has the data-inject CSS property.
type | String | Can be: "DOMATTR", "CSS" or "INNERHTML". Search for only the specific type of response. 
ref | String | Used with CSS & DOMATTR types. Can filter for a specific value type. 

### Return

You can expect a `200 OK` HTTP response code along with an array of json objects describing the inject content possibilities.

## Upload resourcess
Upload resources to the website from an external source. Today, this only supports images, but might be extended in the future for other types of media. This will upload the resource to the CDN that Duda uses and make it available to anyone building the website. This API is commonly used in conjunction with the [inject content API]( #inject-content) to insert new images directly into the website.

### Method and path

`POST /api/sites/multiscreen/resources/{site_name}/upload`

> Input example:

```json
[
  {
    "src": "http://www.dudasupport.com/test/beach.jpg",
    "recource_type": "IMAGE"
  },
  {
    "src": "http://www.dudasupport.com/test/field.jpeg",
    "recource_type": "IMAGE"
  }
]
```

### Input parameters
Duda expects an array of JSON objects that contain a source (src) and resource_type attribute. 

Property | Type | Required |Description
---------- | ---------- | ---------- | ----------
src | URL String | Required | A public URL for the resource. This must be publically available so that Duda can copy it to our CDN / storage associated with the website.
resource_type | String | Required | The type of resource being uploaded. Today, the only allowed value is IMAGE.

> Example

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/published?lastDays=30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '[{"src":"http://www.dudasupport.com/test/beach.jpg","recource_type": "IMAGE"},{"src": "http://www.dudasupport.com/test/field.jpeg","recource_type": "IMAGE"}]'
```
> JSON Return

```json
{
  "n_failures": 0,
  "uploaded_resources": [
    {
      "original_url": "http://www.dudasupport.com/test/beach.jpg",
      "new_url": "https://irt-cdn.multiscreensite.com/a60fe88591064282a20cdb63a8ca5740/beach.jpg",
      "status": "UPLOADED"
    },
    {
      "original_url": "http://www.dudasupport.com/test/field.jpeg",
      "new_url": "https://irt-cdn.multiscreensite.com/a60fe88591064282a20cdb63a8ca5740/field.jpeg",
      "status": "UPLOADED"
    }
  ]
}
```
### Return 

Duda will return a `200 OK` response code along with the following data:

Property | Type | Description
---------- | ---------- | ----------
n_failures | int | The number of failed resource that failed to upload. 
uploaded_resources | Array of Objects | An array of objects describing each resource uploaded. This contains an original URL, new URL (in the Duda system) and a status. 
original_url | URL String | The original URL/soruce of the resource that was uploaded. 
new_url | URL String | A direct URL link to the resource, uploaded to the website. 
status | String | The status of the upload. This can be *NOT_FOUND* if Duda could not access the resource or *UPLOADED* for a successful upload.

<aside class="notice">Since Duda must download the image, multi-size them and finally compress images uploaded through this API, it can often take several seconds to respond, depending on the number of images. If possible, we recommend you perform this action asynchronously.</aside>

## Get recently published sties

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
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/published?lastDays=30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```
## Get recently unpublished sties

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
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/unpublished?lastDays=30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Response

Duda will return a `200 OK` response code along with an array of strings that are Duda Site_name values. 

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
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/get-forms/b4ra2g?from=2017-01-01&to=2017-01-30' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

## Generate SSL Certificate

Generate a SSL certificate for a specific website. This will enable a HTTPS connection between site visitors and the Duda platform. It usually takes 15-30 minutes to successfully generate a SSL certificate for a website. You can get the status of the SSL certificate by calling the [GET site API](#get-site) and checking the certificate_status parameter. Once the certificate is successfully generated, all website visitors will be redirected to the HTTPS connection. This can be disabled on by updating the site force_https value to false. You do not need to worry about renewing the certificate, as Duda will do this automatically.

### Method and path

`POST /sites/multiscreen/{site_name}/certificate`

URI Parameter:
- site_name

### Return

Upon success, Duda will return a `204 No Content` HTTP code.

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/b4ra2g/certificate' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

<aside class="warning">Before sending this API call, it's important that the domain is successfully pointed to Duda's servers. We only currently offer Domain Verified (DV) certificates. As part of the API call, Duda will verify that the domain is correctly pointing at the platform. If it is not, Duda will return an error.</aside>


## Delete SSL Certificate

Delete a certificate that has been generated for a website. This will ensure that the website is served over only an HTTP (insecure) connection and will delete the generated certificate.

### Method and path

`DELETE /sites/multiscreen/{site_name}/certificate`

URI Parameter:
- site_name

### Return

Upon success, Duda will return a `204 No Content` HTTP code.

```shell
curl -X DELETE -k 'https://api.dudamobile.com/api/sites/multiscreen/b4ra2g/certificate' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```
## Create site backup

Create a new backup of a site. This is used for saving the existing state of a site. Good for saving a restore point before a user starts to edit a site or after work has been completed.

<aside class="notice">Each site can have a maximum of 10 manually created backup versions and 20 that are automatically created upon each publish of the site (30 in total). Backups that are automatically created will have a name such as: sitename_Auto_4</aside>

### Method and path

`POST /sites/multiscreen/backups/{site_name}/create`

### Parameters

- site_name

> Example JSON to send:

```json
{
   "name":"QA-Complete"
}
```

### Body Parameters

You can define the name of the backup:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
name | String | Optional | Set the name of the backup you are creating. 

> Example

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/backups/b4ra2g' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"name":"QA-Complete"}''
```
> Returned data:

```json
{
  "backup_name": "QA-Complete"
}
```

<aside class="warning">The name of the backup cannot contain a space.</aside>

### Return

Upon success, Duda will return a `200 OK` HTTP response code along with the backup_name of the created backup. 


## Get site backups

Get an array of existing site backups/versions. A backup can be created from inside of the editor (or by API) and is also automatically created every time the site is published. 

> Example JSON return:

```json
[
   {
      "date":"2014-06-07T11:18:51",
      "name":"russ7_Auto_3"
   },
   {
      "date":"2014-06-07T11:19:02",
      "name":"russ7_Auto_4"
   },
   {
      "date":"2014-06-07T11:19:13",
      "name":"russ7_Auto_5"
   },
   {
      "date":"2014-06-07T11:19:21",
      "name":"russ7_Auto_6"
   },
   {
      "date":"2014-06-07T11:29:49",
      "name":"russ7_Auto_7"
   },
   {
      "date":"2014-06-07T11:33:21",
      "name":"ManualBackup_1"
   }
]
```

### Method and path
`GET /sites/multiscreen/backups/{site_name}`

### URI Parameters:
- site_name

> CURL Example:

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/backups/b4ra2g' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

Upon successful call, Duda will return a `200 OK` response code along with the array of data objects in the body. 

## Restore site

Restore a site from an existing backup. This will fully restore the site back to the state it was in at the time of the backup creation. When restoring a site, a backup is automatically made of the site before restoring the backup. You will be able to see the backup that is created via the Get Site Backups call.

### Method and path

`POST /sites/multiscreen/backups/{site_name}/restore/{backup_name}`

### Parameters

- site_name
- backup_name - Obtained from the get backup name API call

> Example: 

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/backups/b4ra2g/restore/QA-Complete' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

Upon success, Duda will return a `204 No Content` HTTP status code. 

## Delete site backup

 Permanently delete a site backup. There is no way to restore or gain access to deleted site backups.

### Method and path

`DELETE /sites/multiscreen/backups/{site_name}/restore/{backup_name}`

### Parameters

* site_name
* backup_name - Obtained from the get backup name API call

> Example: 

```shell
curl -X DELETE -k 'https://api.dudamobile.com/api/sites/multiscreen/backups/b4ra2g/restore/QA-Complete' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

Upon success, Duda will return a `204 No Content` HTTP status code.
