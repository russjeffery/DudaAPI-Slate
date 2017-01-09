# Multiscreen Sites

A site instance resource represents a single Duda site. There are two main types of Duda sites: mobile and multiscreen. Mobile represents a DudaMobile site and multiscreen represents a Responsive site. Below, you will see the API resources available for the multiscreen (DudaOne) sites only.

<aside class="notice">Pewpew</aside>

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
			"opentable_info":[
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
url | String | optional | Have Duda import content into the new site based off of an existing website. You may use an existing website address or Facebook business page. 
default_domain_prefix | String | optional | Set the default sub-domain for your site. This controls the <sub-domain-prefix>.dudaone.com address. 
site_data | Object | optional | JSON object containing additional site data. See below for the options and properties.
site_alternate_domain | Object | optional | Set alternate domains for the site. The alternate domain values will be redirected to the primary domain (set in the site_data object). 

The description for the site_data object is below:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
site_domain | String | optional | Set the domain of the site. This will be: www.example.com. This will be used when the site is published. This must be unique inside of the Duda platform. 
external_uid | string | optional | A flexible string you pass to Duda when creating the site, allows you to store your customer's unique value to reference later (45 char max). Note that Duda does not enforce uniqueness here.
business_site_info | Object | optional | Pass Duda detailed information about this business / website.

###site_business_info object
Provides structured data about a website or business. Duda will use this information to help populate the website with this data and fill the [content library](https://help.dudamobile.com/hc/en-us/articles/229790908-Manage-Content-Library) with data. All of the data fields are optional.

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
Street | String | A street level line location (e.g. 123 Main Strett)
State | String | The state of where the business is located
Country | String | The country of where the business is located
zip_code | String | The Zip Code or Postal Code of the business

### OpenTable object
Providing this will create an OpenTable button and help Duda find more information about the website online. This should be an array of JSON objects, as you can provide multiple restaurant_id's while creating the site.

Property | Type | Description
---------- | ---------- | ----------
restaurant_id | String | The unique OpenTable Restaurant ID. (e.g. 55123)
location | String | A description of the location (e.g. Downtown)
country | String | Two digit country location (e.g. US, DE, JA)

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

## Get Site
Get information about an already existing website. You will need the site_name value that was provided during the create website call. 

<aside class="warning">Note that only properties that have values will be returned.</aside>

`GET /sites/multiscreen/{site_name}`

### Required URI Parameters
site_name - URL Prameter. Originally returned when first creating a website.

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
preview_site_url | String | A direct URL to a the three-screen preview of the website in it's current state while in design. This white labeled URL non-authenticated and can be accessed by anyone online
last_published_date | date time | The date of the most recent publication of the website. 
first_published_date | date time | The date of when this website was first published. Note that if you unpublish the website and republish it, the first_published_date will not change
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

## Get Sites by External ID
This will return a list of all site ID's that have the external ID value provided  when creating or updating the website. The external ID value is not a unique value in the Duda system -- so multiple sites can have the same external ID value. 

`GET /sites/multiscreen/byexternalid/{external_uid}`

### Parameters

- external_uid - URL parameter

### Return

Duda will return an array of site_names for each website that has the matching external ID value. 

You should expect a 200 HTTP responce code with the array of site names in the body of the response. 

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

- Site Name - URI Prameter

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

### Prameters to send
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

## Delete Site
This will immediately and permanently delete the site and cancel any subscription associated with the site. Please note that after deleting a site there is no way to bring the site back. Deleting a site will also cancel any active subscription/payment associated with the site. 

### Method and path

`DELETE /sites/multiscreen/{site_name}`

### Parameters
- Site Name: URI Prameter

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

- Site Name - URI Prameter

> CURL Example:

```shell
curl -X POST -k 'https://api.dudamobile.com/api/sites/multiscreen/publish/57b6506a' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'	
```

### Return
Duda will return a `204 No Content` HTTP code for successful calls. 

## Unpublish site
Removes the site from the production environment. This is good for disabling the site or taking it down temporarily so no one can access it. This will not delete the website. The website will still exist adn can be published later. 

### Method and path
`POST /sites/multiscreen/unpublish/{site_name}`

### Parameters

- Site Name - URI Prameter

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

The following prameters can be sent in the body for the new website that will be created. They are all optional

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

Duda will return a sites object with an array of sites. The details of then site values returned will be the exact same as the [get site example above](#get-site), please see there for details about values returned.

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

### Optional URI Prameter

You can add a `?lang=en` URL prameter onto the template API path call to get the templates for specific languages. You can see the languages available via the API here.

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

You can expect a `200 OK` response along with the all of the template data (in an array of objects) shown here.

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

### Optional URI Prameter

You can add a `?lang=en` URL prameter onto the template API path call to get the templates for specific languages. You can see the languages available via the API here

> CURL Example:

```shell
curl -X GET -k 'https://api.dudamobile.com/api/sites/multiscreen/templates/20012' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

You an expect a `200 OK` response along with an JSON object that describes the given template. 

## Create template from site

Take an existing website and turn it into a template. This template can then be used to build new sites from. The template will be returned as part of the GET TEMPLATES api call and also be available to select from while creating a new site (under the My Templates section while creating a new multiscreen site).

### Method and path

`POST /sites/multiscreen/template/fromsite`

### Parameters

Send the following 

