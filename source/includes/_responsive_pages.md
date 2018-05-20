## Get pages

Get all pages that exist within the website. Returns an array of objects that describes each page of the site. 

### Method and path

`GET /api/sites/multiscreen/site/{site_name}/pages`

> CURL Example:

```shell
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/site/b4ra2g/pages' \
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
curl -X POST -k 'https://api.duda.co/api/sites/b4ra2g/pages/Y29udGFjdA/update' \
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
curl -X GET -k 'https://api.duda.co/api/sites/b4ra2g/pages/YWJvdXQvYWJhYg' \
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
curl -X DELETE -k 'https://api.duda.co/api/sites/b4ra2g/pages/YWJvdXQvYWJhYg' \
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
