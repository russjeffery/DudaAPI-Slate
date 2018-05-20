## Get Content Library Data

> JSON Data Returned:

```json
{
    "location_data": {
        "phones": [
            {
                "phoneNumber": "123-123-1234",
                "label": "Russ Phone"
            },
            {
                "phoneNumber": "18001234567",
                "label": "Duda Phone"
            }
        ],
        "emails": [
            {
                "emailAddress": "api@duda.co",
                "label": "API Email"
            },
            {
                "emailAddress": "support@duda.co",
                "label": "Support Email"
            }
        ],
        "label": "Duda HQ",
        "social_accounts": {
            "tripadvisor": "Restaurant_Review-g32849-d2394400-Reviews-Oren_s_Hummus_Shop-Palo_Alto_California.html",
            "youtube": "UCPMwzOc1Su-s2z-J1xiU9ig",
            "facebook": "duda",
            "yelp": "orens-hummus-shop-palo-alto",
            "pinterest": "michelleobama",
            "google_plus": "+Dudamobile577",
            "linkedin": "duda",
            "instagram": "orenshummus",
            "snapchat": "michelleobama",
            "twitter": "dudamobile",
            "rss": "https://www.duda.co/blog/feed/",
            "vimeo": "dudamobile",
            "reddit": "duda"
        },
        "address": {
            "streetAddress": "577 College Ave",
            "postalCode": "94306",
            "city": "Palo Alto",
            "country": "US"
        },
        "logo_url": "https://du-cdn.multiscreensite.com/duda_website/img/home/agencies.svg",
        "business_hours": [
            {
                "days": [
                    "SAT",
                    "SUN"
                ],
                "open": "00:00",
                "close": "00:00"
            },
            {
                "days": [
                    "MON",
                    "TUE",
                    "WED",
                    "THU",
                    "FRI"
                ],
                "open": "09:00",
                "close": "18:00"
            }
        ]
    },
    "additional_locations": [
        {
            "uuid": "276169839",
            "phones": [
                {
                    "phoneNumber": "123-123-1234",
                    "label": ""
                }
            ],
            "emails": [],
            "label": "Duda Tel Aviv",
            "social_accounts": {},
            "address": {},
            "geo": {
                "longitude": "34.78337",
                "latitude": "32.07605"
            },
            "logo_url": null,
            "business_hours": null
        }
    ],
    "site_texts": {
        "overview": "Oh, Duda? Duda is a variation of \"Dude\", who just happens to be the main character in one of our favorite movies of all time: The Big Lebowski. You should watch it some time. Look out for that ferret!",
        "services": "- Responsive Website Builder",
        "custom": [
            {
                "label": "Example CTA 1",
                "text": "THE WEB DESIGN PLATFORM FOR Scaling Your Agency"
            },
            {
                "label": "Example CTA 2",
                "text": "THE WEB DESIGN PLATFORM FOR\nBuilding Websites Faster"
            }
        ],
        "about_us": "Duda is a leading website builder for web professionals and agencies of all sizes. Our website builder enables you to build amazing, feature-rich websites that are perfectly suited to desktop, tablet and mobile. Our mobile builder enables you to build mobile-only sites from scratch, or based on an existing desktop site or Facebook business page. Duda allows professionals and agencies to build high-converting, personalized websites at scale. Duda optimizes each and every site for Google PageSpeed."
    },
    "business_data": {
        "name": "Duda",
        "logo_url": "https"
    }
}
```

Get the data that exists within the content library of a website. The content library is a central data stores of the website that stores structured information. This information can be used to help assist in the site build by providing accurate information that is available within the website builder directly. 

### Method and path
`GET /api/sites/multiscreen/{site_name}/content`

### Parameters

- site_name

### Location Properties Returned

Below is the list of possible location specific data that will be returned via the API. Note that this data can be returned for the primary location (`location_data`) or any secondary/child locations (`additional_locations`). All fields are optional.

Property | Type | Description
---------- | ---------- | ----------
location_data | object | Represents the primary location of this business. It contains all location specific data points and should be used for all cases exepct where there is more than one location for this business/website. 
phones | Array of objects | Contains phone numbers for this location. The object allows for a friendly label name and the actual phone number. Max 80 characters each.
emails | Array of objects | Contains all email addresses associated with this location. The object allows for a friendly label name and the acutal email address. Max 80 characters each.
label | String | A simple name for this location. This is displayed in the Duda Content Library UI related to this location. Max 80 characters.
social_accounts | Object | The profile name of this locations social networks. You must pass only the profile name/ID. Do not pass the full URL (e.g. https://wwwfacebook.com/duda). We support the following social networks: Facebook, Twitter, Yelp, Foursquare, Google Plus, Instagram, Youtube, Linkedin, Pinterest, Vimeo, RSS, Reddit, Trip Advisor & Snapchat. 
address | Object | Contains all fields required to display an address: streetAddress, postalCode, region, city, country. 
logo_url | String | A URL directly referencing the logo of this location. Must be a public URL and be served over HTTPS. 
business_hours | Array of Objects | An array containing each day of the week and the hours that the location opens and closes. For each set of hours, you can pass an array of days: MON, TUE, WED, THU, FRI, SAT, SUN that apply to those hours. Open and close hours must be in 24HH:MM format. So, for example, 7:30 am would be: 07:30 and 5 pm would be 17:00. 

### Global Properties Returned

Below is website content that does not relate to a specific location.

Property | Type | Description
---------- | ---------- | ----------
site_texts | Object | An Object containing overview, services, about_us and custom text strings. Each field has a max length of 2000 characters. 
custom | Array of Objects | An array that contains custom text strings. These will each have a label & text associated with them. These can be used for storing site data or information about the business.
business_data | Object | Set the `name` of the business and the primary image `logo_url` of the website. 


### Return
You can expect a `200 OK` HTTP code upon success.

<aside class="notice">The content library API has fields which do not currently display within the content library in the Duda editor. Examples of this are: Business Hours, Logo, Business Name, etc.. We plan to update this, but this is currently a small inconsistency in the platform.</aside>

## Update content library data

> JSON Body to send:

```json
{
    "location_data": {
        "phones": [
            {
                "phoneNumber": "123-123-1234",
                "label": "Russ Phone"
            },
            {
                "phoneNumber": "18001234567",
                "label": "Duda Phone"
            }
        ],
        "emails": [
            {
                "emailAddress": "api@duda.co",
                "label": "API Email"
            },
            {
                "emailAddress": "support@duda.co",
                "label": "Support Email"
            }
        ],
        "label": "Duda HQ",
        "social_accounts": {
            "twitter": "dudamobile",
            "facebook": "duda",
            "youtube":"UCPMwzOc1Su-s2z-J1xiU9ig",
            "yelp":"orens-hummus-shop-palo-alto",
            "fourSquare":"4df7ce8315205e0e3f6b06a4",
            "linkedin":"duda",
            "google_plus":"+Dudamobile577",
            "instagram":"orenshummus",
            "pinterest":"michelleobama",
            "vimeo":"dudamobile",
            "rss":"https://www.duda.co/blog/feed/",
            "reddit":"Duda",
            "tripadvisor":"Restaurant_Review-g32849-d2394400-Reviews-Oren_s_Hummus_Shop-Palo_Alto_California.html",
            "snapchat":"michelleobama"
        },
        "address": {
        	"streetAddress":"577 College Ave",
        	"postalCode":"94306",
        	"region":"",
        	"city":"Palo Alto",
        	"country":"US"
        },
        "logo_url": "https://du-cdn.multiscreensite.com/duda_website/img/home/agencies.svg",
        "business_hours": [
        	{
        		"days": ["SAT","SUN"],
	        	"open": "00:00",
	        	"close": "00:00"
        	},
        	{
        		"days":["MON","TUE","WED","THU","FRI"],
        		"open":"09:00",
        		"close":"18:00"
        	}
        	]
    },
    "site_texts": {
        "overview": "Oh, Duda? Duda is a variation of \"Dude\", who just happens to be the main character in one of our favorite movies of all time: The Big Lebowski. You should watch it some time. Look out for that ferret!",
        "services": "- Responsive Website Builder",
        "custom": [
            {
                "label": "Example CTA 2",
                "text": "THE WEB DESIGN PLATFORM FOR\nBuilding Websites Faster"
            },
            {
                "label": "Example CTA 1",
                "text": "THE WEB DESIGN PLATFORM FOR Scaling Your Agency"
            }
        ],
        "about_us": "Duda is a leading website builder for web professionals and agencies of all sizes. Our website builder enables you to build amazing, feature-rich websites that are perfectly suited to desktop, tablet and mobile. Our mobile builder enables you to build mobile-only sites from scratch, or based on an existing desktop site or Facebook business page. Duda allows professionals and agencies to build high-converting, personalized websites at scale. Duda optimizes each and every site for Google PageSpeed."
    },
    "business_data": {
        "name": "Duda",
        "logo_url": "https"
    }
}
```

>Simple CURL to update business hours:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/57b6506a/content' \
    -u 'APIusername:APIpassword' \ 
    -H 'Content-Type: application/json' \
    -d '{"location_data": {"business_hours": [{"days": ["SAT","SUN","MON"],"open": "00:00","close": "00:00"},{"days":["TUE","WED","THU","FRI"],"open":"09:00","close":"17:30"}]}}'
```

Update the content of the content that exists within the content library. This will update the data that exists within the content library and make it available during the site builds. For example, when adding a map, Duda will automatically populate the address from the content library. 

The data you can set is the exact same that you get from the content library API call.

### Method and path
`POST /api/sites/multiscreen/{site_name}/content`

### Parameters

- site_name

## Create additional location

> JSON Body to send:

```json
{
    "phones": [
        {
            "phoneNumber": "123-123-4321",
            "label": "Main Phone"
        },
        {
            "phoneNumber":"123-123-5321",
            "label":"Scheduling"
        }
    ],
    "emails": [
        {
            "emailAddress":"colorado@duda.co",
            "label":"Colorado Office Email"
        }
    ],
    "label": "Duda Colorado - Goosetail Labs",
    "social_accounts": {
        "facebook":"duda",
        "twitter":"dudamobile"
    },
    "address": {
        "streetAddress":"197 S 104th St C",
        "city":"Louisville",
        "postalCode":"80027",
        "country":"USA"
    },
    "logo_url": "https://www.duda.co/developers/REST-API-Reference/images/duda.svg",
    "business_hours":  [
            {
                "days": [
                    "SUN",
                    "SAT"
                ],
                "open": "00:00",
                "close": "00:00"
            },
            {
                "days": [
                    "MON",
                    "TUE",
                    "WED",
                    "THU",
                    "FRI"
                ],
                "open": "09:00",
                "close": "18:00"
            }
        ]
}
```

> Example URL to send:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/content/location' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"phones":[{"phoneNumber":"123-123-4321","label":"Main Phone"},{"phoneNumber":"123-123-5321","label":"Scheduling"}],"emails":[{"emailAddress":"colorado@duda.co","label":"Colorado Office Email"}],"label":"Duda Colorado - Goosetail Labs","social_accounts":{"facebook":"duda","twitter":"dudamobile"},"address":{"streetAddress":"197 S 104th St C","city":"Louisville","postalCode":"80027","country":"USA"},"logo_url":"https://www.duda.co/developers/REST-API-Reference/images/duda.svg","business_hours":[{"days":["SUN","SAT"],"open":"00:00","close":"00:00"},{"days":["MON","TUE","WED","THU","FRI"],"open":"09:00","close":"18:00"}]}''
```

> JSON Response below. Note the UUID.

```json
{
    "uuid": "367163873",
    "phones": [
        {
            "phoneNumber": "123-123-4321",
            "label": "Main Phone"
        },
        {
            "phoneNumber": "123-123-5321",
            "label": "Scheduling"
        }
    ],
    "emails": [
        {
            "emailAddress": "colorado@duda.co",
            "label": "Colorado Office Email"
        }
    ],
    "label": "Duda Colorado - Goosetail Labs",
    "social_accounts": {
        "twitter": "dudamobile",
        "facebook": "duda"
    },
    "address": {
        "streetAddress": "197 S 104th St C",
        "postalCode": "80027",
        "city": "Louisville",
        "country": "USA"
    },
    "geo": null,
    "logo_url": "https://www.duda.co/developers/REST-API-Reference/images/duda.svg",
    "business_hours": [
        {
            "days": [
                "SUN",
                "SAT"
            ],
            "open": "00:00",
            "close": "00:00"
        },
        {
            "days": [
                "MON",
                "TUE",
                "WED",
                "THU",
                "FRI"
            ],
            "open": "09:00",
            "close": "18:00"
        }
    ]
}
```

Create a new location for this website. This location will be apart of the `additional_locations` data that is returned from the Get Content Library API call. If you are aligning your location management system with this website, we recommend storing both the site name & the UUID of the location in your database.

### Method and path
`POST /api/sites/multiscreen/{site_name}/content/location`

### URI Parameters
- Duda expects the exiting {site_name} to be sent in the URI

### Body Parameters
- Duda expects a JSON object describing the location details. You can see the full representation to the right. 

### Response
A successful create location API call will return a `200 OK` HTTP response with all the location data you just submitted as the body. This body will also contain the UUID of this location

## Get Location Data

> Example CURL to send

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/content/location/367163873' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

> JSON Response:

```json
{
    "uuid": "367163873",
    "phones": [
        {
            "phoneNumber": "123-123-4321",
            "label": "Main Phone"
        },
        {
            "phoneNumber": "123-123-5321",
            "label": "Scheduling"
        }
    ],
    "emails": [
        {
            "emailAddress": "colorado@duda.co",
            "label": "Colorado Office Email"
        }
    ],
    "label": "Duda Colorado - Goosetail Labs",
    "social_accounts": {
        "twitter": "dudamobile",
        "facebook": "duda"
    },
    "address": {
        "streetAddress": "197 S 104th St C",
        "postalCode": "80027",
        "city": "Louisville",
        "country": "USA"
    },
    "geo": {
        "longitude": null,
        "latitude": null
    },
    "logo_url": "https://www.duda.co/developers/REST-API-Reference/images/duda.svg",
    "business_hours": [
        {
            "days": [
                "SUN",
                "SAT"
            ],
            "open": "00:00",
            "close": "00:00"
        },
        {
            "days": [
                "MON",
                "TUE",
                "WED",
                "THU",
                "FRI"
            ],
            "open": "09:00",
            "close": "18:00"
        }
    ]
}
```
Get a specific locations data. To get a specific locations details, you need the UUID of that location. You can find the location `uuid` in the Get Content Library Data or after you've created a location. Note that you can only update `additional_locations` via this API Call.

### Method and path
`GET /api/sites/multiscreen/{site_name}/content/location/{uuid}`

### Parameters
- Duda requires you pass the `site_name` and location `uuid` inside of the URI to get the location specific data

### Return
A successfull call will return a `200 OK` response, with a JSON object explaining the data that exists within the location (seen to the right).

## Update Location
Update an existing location within the content library. You can only update `additional_locations` that exist as part of the content library. 

> CURL Example:

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/content/location/367163873' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"phones":[{"phoneNumber":"999-999-1234","label":"Main Phone"},{"phoneNumber":"123-123-1235","label":"Scheduling"}]}''
```

### Method and path
`POST /api/sites/multiscreen/{site_name}/content/location/{uuid}`

### Data parameters
- In the URI, you need to set the `site_name` and `uuid` of this location. 
- The body should be a valid content library parameter (documented above).

### Return
Duda will return a `204 No Content` HTTP response code.

<aside class="warning">When you update values of the data, make sure you're updating the entire property. For example, if you update the phone, make sure you send back all valid phone numbers that exist for the location and don't just update a specific phone number.</aside>

## Delete location
Delete an existing location. You will need the specific UUID of this location. 

>CURL Example:

```shell
curl -X DELETE -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/content/location/367163873' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
```

### Method and path
`DELETE /api/sites/multiscreen/{site_name}/content/location/{uuid}`

### Data parameters
- In the URI, you need to set the `site_name` and `uuid` of this location. 

### Return
Duda will return a `204 No Content` HTTP response code.

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

This feature primarily works by sending the `type` value to alter the markup or styling in multiple ways. Should pass Duda an array of JSON objects. For a full example of content injection.


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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/inject-content/b4ra2g' \
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
-X GET -k 'https://api.duda.co/api/sites/multiscreen/inject-content/b4ra2g' \
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
-X GET -k 'https://api.duda.co/api/sites/multiscreen/inject-content/b4ra2g?type=INNERHTML' \
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

## Upload resources
Upload resources to the website from an external source. Today, this only supports images, but might be extended in the future for other types of media. This will upload the resource to the CDN that Duda uses and make it available to anyone building the website. This API is commonly used in conjunction with the [inject content API]( #inject-content) to insert new images directly into the website.

<aside class="warning">Please make sure files you upload have the correct extension. For example, PNG images should have .png and JPEG should have .jpg or .jpeg.</aside>

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
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/published?lastDays=30' \
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