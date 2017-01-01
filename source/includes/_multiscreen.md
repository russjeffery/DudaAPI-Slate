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
external_uid | string | optional | A flexible string you pass to Duda when creating the site, allows you to store your customer's unique value to reference later. (45 char max)
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
Upon successful creation of a website, Duda will return a site_name. This is a unique value across all Duda websites and will be needed to later access other resources, such as updating the site, getting analytics or granting access. We recommend storing this site_name in your database for later reference.

## Get Site
Get information about an already existing website. You will need the site_name value that was provided during the create website call. 

`GET /sites/multiscreen/{site_name}`

### Required URI Parameters
site_name - URL Prameter


