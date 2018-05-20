## Get all templates

Get an array of objects for all templates available to your account. Each template has a set of data associated with it, including the template_id, which is required to create new sites from. Both Duda default templates and custom templates that you create will be returned by this API call. 

### Method and path

`GET /sites/multiscreen/templates`

> Example of returned template data:

```json
[
	{
        "template_name": "Auto Repair",
        "preview_url": "http://dashboard.russjeffery.com/preview/dm-theme-1005442-en-328",
        "thumbnail_url": "https://irp-cdn.multiscreensite.com/0f8a84df3a244e0f9249423e0588397f/siteTemplateIcons/IzrGRlYSCeQNUc4V1nAA_Auto_repair_BigPreview.png",
        "desktop_thumbnail_url": "https://irp-cdn.multiscreensite.com/0f8a84df3a244e0f9249423e0588397f/siteTemplateIcons/GwrdUIcXTbOypxHaiUNZ_auto_repair_desktop.png",
        "tablet_thumbnail_url": "https://irp-cdn.multiscreensite.com/0f8a84df3a244e0f9249423e0588397f/siteTemplateIcons/LUihe1rSq5kiizBraMgG_auto-repair_ipad.png",
        "mobile_thumbnail_url": "https://irp-cdn.multiscreensite.com/0f8a84df3a244e0f9249423e0588397f/siteTemplateIcons/rcceNsKOTKSqSx5bnPME_auto-repair-mobile.png",
        "template_id": 1005442,
        "template_properties": {
            "can_build_from_url": false,
            "has_store": false,
            "has_blog": false,
            "page_count": 5
        }
    },
    {
        "template_name": "Flower Store",
        "preview_url": "http://dashboard.russjeffery.com/preview/dm-theme-1004110-en-324",
        "thumbnail_url": "https://irp-cdn.multiscreensite.com/aee0f8c56d634e6f969aeed4c5bc9fc4/siteTemplateIcons/R8gW4rpREyJVcyodUBqW_BigPreview.jpg",
        "desktop_thumbnail_url": "https://irp-cdn.multiscreensite.com/aee0f8c56d634e6f969aeed4c5bc9fc4/siteTemplateIcons/eYs4FiVQkmUoOyxxNcPd_flower_store_desktop.png",
        "tablet_thumbnail_url": "https://irp-cdn.multiscreensite.com/aee0f8c56d634e6f969aeed4c5bc9fc4/siteTemplateIcons/NK8oQFkOTEG0FbHqhqXX_flower_store_tablet.png",
        "mobile_thumbnail_url": "https://irp-cdn.multiscreensite.com/aee0f8c56d634e6f969aeed4c5bc9fc4/siteTemplateIcons/tW6yISIT5yHHRg0oakt7_flower_store_mobile.png",
        "template_id": 1004110,
        "template_properties": {
            "can_build_from_url": false,
            "has_store": true,
            "has_blog": false,
            "page_count": 3
        }
    },
    {
        "template_name": "Blank Services",
        "preview_url": "http://dashboard.russjeffery.com/preview/dm-theme-1003738-en-327",
        "thumbnail_url": "https://irp-cdn.multiscreensite.com/fac18167cc14456196ba71cef32d4bc0/siteTemplateIcons/fViD6lOeQZmv89Yvf7vM_Blank_services_BigPreview.jpg",
        "desktop_thumbnail_url": "https://irp-cdn.multiscreensite.com/fac18167cc14456196ba71cef32d4bc0/siteTemplateIcons/JcFd47BRSmOv8utqovg3_blank_services_desktop.png",
        "tablet_thumbnail_url": "https://irp-cdn.multiscreensite.com/fac18167cc14456196ba71cef32d4bc0/siteTemplateIcons/Mva1521TEK3O936gia2F_blank_services_tablet.png",
        "mobile_thumbnail_url": "https://irp-cdn.multiscreensite.com/fac18167cc14456196ba71cef32d4bc0/siteTemplateIcons/xNEqNg6ZR56eI3ngAxvE_blank_services_mobile.png",
        "template_id": 1003738,
        "template_properties": {
            "can_build_from_url": false,
            "has_store": false,
            "has_blog": false,
            "page_count": 3
        }
    },
    {
        "template_name": "Conference",
        "preview_url": "http://dashboard.russjeffery.com/preview/dm-theme-1004580-en-320",
        "thumbnail_url": "https://irp-cdn.multiscreensite.com/c3e028f9d55f47dfbde4c4c237ffda9f/siteTemplateIcons/LgJaTshSpaGY709UMa5f_BigPreview%20(2).jpg",
        "desktop_thumbnail_url": "https://irp-cdn.multiscreensite.com/c3e028f9d55f47dfbde4c4c237ffda9f/siteTemplateIcons/10xwaGtIQX6GQziGsTXs_conference_desktop.png",
        "tablet_thumbnail_url": "https://irp-cdn.multiscreensite.com/c3e028f9d55f47dfbde4c4c237ffda9f/siteTemplateIcons/RezGoJXWQWWbOkd73RBI_conference_tablet.png",
        "mobile_thumbnail_url": "https://irp-cdn.multiscreensite.com/c3e028f9d55f47dfbde4c4c237ffda9f/siteTemplateIcons/r4gs9oeLQz8JItdoilAL_conference_mobile.png",
        "template_id": 1004580,
        "template_properties": {
            "can_build_from_url": false,
            "has_store": false,
            "has_blog": false,
            "page_count": 3
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
mobile_thumbnail_url | URL String | A screenshot of the mobile view of this template.
desktop_thumbnail_url | URL String | A screenshot of the desktop view of this template.
tablet_thumbnail_url | URL String | A screenshot of the tablet view of this template.
template_id | Int | A unique number associated with this template. This is used to create the template from. 
template_properties | Object | Contains proprieties of the templates. Currently this only contains can_build_from_url
can_build_from_url | Bool | Will be either true or false. If it is false, this means that the template is a custom template which cannot pull content from an external URL while creating the site. 
has_store | bool | If the template has an eCommerce store built into it.
has_blog | bool | If the template has a blog added to the website once it's created.
page_count | int | The number of pages this template contains.

> CURL Example to Get All Templates:

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/templates' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

You can expect a `200 OK` response along with the all the template data (in an array of objects) shown here.

<aside class="notice">Only templates built by Duda will have a tablet or mobile screenshot returned in the API. Duda does not currently have a way to generate mobile & tablet preview screenshots automatically.</aside>

## Get template info

Get the name, preview URL, thumbnail_url, and template ID of a single template. You can use this to get particular images or preview URLs of templates to display to your users. This will allow them to see the variety of templates to choose from. If you want to see the full list of template ID's available, please use the [get all templates call](#get-all-templates).

> JSON Data Returned:

```json
{
	"template_name": "Consultant Landing Page",
    "preview_url": "http://dashboard.russjeffery.com/preview/dm-theme-1026287-en-343",
    "thumbnail_url": "https://irp-cdn.multiscreensite.com/c1e7738d32ae46538527c1fd4e95a9b9/siteTemplateIcons/4CXbg61CTvKDPTEyC7EQ_Consultant_landing_page_3%20screens.png",
    "desktop_thumbnail_url": "https://irp-cdn.multiscreensite.com/c1e7738d32ae46538527c1fd4e95a9b9/siteTemplateIcons/jASytIEDRJWRG0dxSbJj_Consultant_landing_page_Desktop.png",
    "tablet_thumbnail_url": "https://irp-cdn.multiscreensite.com/c1e7738d32ae46538527c1fd4e95a9b9/siteTemplateIcons/4XZxi7HCTWaPx7oTc6fW_Consultant_landing_page_Tablet.png",
    "mobile_thumbnail_url": "https://irp-cdn.multiscreensite.com/c1e7738d32ae46538527c1fd4e95a9b9/siteTemplateIcons/8V63M24zRYaJG96sWSdu_Consultant_landing_page_Mobile.png",
    "template_id": 1026287,
    "template_properties": {
        "can_build_from_url": false,
        "has_store": false,
        "has_blog": false,
        "page_count": 1
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
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/templates/1026287' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

You can expect a `200 OK` response along with an JSON object that describes the given template. 

<aside class="notice">Only templates built by Duda will have a tablet or mobile screenshot returned in the API. Duda does not currently have a way to generate mobile & tablet preview screenshots automatically.</aside>

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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/template/fromsite' \
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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/template/fromtemplate' \
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
curl -X DELETE -k 'https://api.duda.co/api/sites/multiscreen/template/100041' \
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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/template/100041' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json' \
	-d '{"new_name":"New template name"}'
```
### Return 

Upon successful update a `204 No Content` HTTP response code will be returned.