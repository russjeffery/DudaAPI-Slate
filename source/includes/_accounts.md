unt_name":"email@email.com", "first_nam# Account

An account resource represents a single Staff or Customer account. There are three account types referenced below, master account, Staff account and Customer account. The master account is the Duda account that accesses the Duda API.

## Create account

Create a new Duda account in which you can grant a customer or staff permissions or site access. This generally will relate back to a customer or staff user from your control panel. 

### Method and path
`POST /accounts/create`

> JSON example to send:

```json
{
	"account_name": "uniqueAcctReference",
	"first_name": "Joann",
	"last_name": "Smith",
	"lang": "en",
	"email": "example@domain.com",
	"account_type": "CUSTOMER"
}
```

> cURL Example

```shell 
curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' \
-X POST -i -k https://api.dudamobile.com/api/accounts/create \
-d '{"account_name":"email@email.com", "first_name":"john", "last_name":"doe", "account_type":"CUSTOMER", "lang":"en"}'	
```

### Parameters:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
account_name | String | Required | Usually the email or unique identifer of the user you want to add. Is not required to be a email. 
first_name | String | Optional | First name off the account holder. This is only used if Duda sends white labeled emails to the customer or the user writes a blog post.
last_name | String | Optional | Last name of the account holder. This is used if Duda sends white labeled emails to the customer or the user writes a blog post.
account_type | String | Optional | Can be "CUSTOMER" or "STAFF". Accounts created are defaulted to customer accounts. 
lang | String | Optional | [Two digit language code](#langauges). Sets what language the user will see the website builder interface in. 

### Return

You can expect a `204 No Content` response code for a successful create account call.

## Get account 

Get information from the Duda platform about an existing account. You should know the Account name already, as you used it to originally create the Account. 

### Method and path

`GET /accounts/{account_name}`

### Parameters

- account_name - URL parameter - account name

> Returned JSON

```json
{
	"account_name": "johnl2@yahoo.com"
	"first_name": "John",
	"last_name": "Lewis",
	"email": "johnl@yahoo.com",
	"account_type":"CUSTOMER"
}
````

### Return:
Upon success, Duda will return a `200 OK` HTTP status code. A JSON object representing the Account will be returned with the following possible values:

Property | Type | Description
---------- | ---------- | ----------
account_name | String | The account name, usually an email address. This is the unique reference to the account.
first_name | String | First name of user. 
last_name | String | Last name of user.
email | Valid Email | Email address of the account. Can be different than account_name
account_type | String | Can be "CUSTOMER" or "STAFF". Represents the type of account.
lang | String | The current language of the account.


> CURL Example:
```shell
curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com
```
## Delete account

Delete an existing Sub-account. This account must be a sub-account you've already created. 

### Method and path

`DELETE /accounts/{account_name}`

### Parameters:

- account_name - URL parameter - Account Name

### Return
 
You can expect a `204 No Content` response code for a successful delete account call.

```shell
curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' \
-X DELETE -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com
```
## Update account

Update the parameters for an existing account. This is useful for updating email, name or language settings. 

### Method and path 
`POST /accounts/update/{account_name}`

> cURL Example
```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.dudamobile.com/api/accounts/update/johnsmith@gmail.com \
-d '{"email": "jsmith@gmail.com"}'
```

### Required Parameters:

- account_name - URL parameter

You can update any property documented in the [create account call](#create-account) on an account when passing data. You must send a valid JSON object to update an account successfully. 

### Return

You can expect a `204 No Content` response code for a successful delete account call.

## Generate SSO link
Generate a Single Sign-On link that will send a user to a specific page in the platform. You can generate a link to: The dashboard, the editor, stats page or to reset a site. If you are sending the user to the stats, editor or reset pages, you will need to pass the site_name and target as a parameter on the request URL. After generating the link, you should direct the users browser to this URL.

The link you generate will have a valid SSO token appended to it which will be valid for two minutes. 

If you are generating a link to a specific site for a customer, please make sure the customer has been granted access to the site and also has appropriate permissions. For example, if the customer does not have access to statistics, then you will be unable to generate an SSO link to the stats page.  

> cURL Example:

```shell
curl -S -u 'APIusername:APIpassword' \ 
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/sso/johnsmith@gmail.com/link​?target=STATS&site_name=146856ab
```

### Method and path
`GET /accounts/sso/{account_name}/link?TARGET=EDITOR&site_name=h5de912a`

### Paramters

You must send a valid account_name as part of the URI parameter. There are several optional URL query paramters you can add on the request. If you pass the stats, Site Editor or Reset Site targets, you must also add a site_name parameter for the website that you are wanting to give the user access to. 

> JSON Response:

```json
{  
  "url": "http://example.mobilewebsiteserver.com/home/site/146856ab?dm_sso=2!eyJyZXFVdWlkIjoiYzU0Y2E0MzYwNDA2NDgwZDlmZmM2MTIxY2FiOTM2MzAifQ"
}
```

Target Location | Query Key | Query Value | Description
---------- | ---------- | ---------- | ----------
Dashboard | (none) | (none) | Send the user directly to their website dashboard. You do not need to send a target or site_name parameter.
Stats | "TARGET" | "STATS" | Send the user directly to the website statistics page. 
Site Editor | "TARGET" | "EDITOR" | Send the user directly to the white labeled website builder.
Stats | "TARGET" | "RESET_SITE" | Send the user directly to the select new template / reset site page. Note that if the user does reset the site, all existing website edits will be lost and the site will be reset to a new template that is selected. 

## Generate single sign-on token

> cURL Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/sso/johnsmith@gmail.com/token​
```

<aside class="notice">We recommend using the generate SSO Link API instead of the Generate SSO Token. It is easier to use.</aside>

Generate a token that will grant sub-accounts access to your white label portal. By calling this API, Duda will return a token that you can be append to the end of your SSO Endpoint in order to authenticate a sub-account directly into your white label account. To generate a Single Sign-On Token for a specific site, you must first give access to a specific site for the Sub-account you want to access that site. Duda will return a URL Parameter with a key and a value. Use this key value pair to append the Token on to the SSO Endpoint URL.

### Method and path
`GET /accounts/sso/{account_name}/token`

### Parameters:</strong> 

- account_name - URL Parameter - Account name

### Return:

HTTP Code: 200 OK 

> JSON Response example:

```json
{  
	"url_parameter" : {  
		"name" : "dm_sso",
		"value" : "2|eyJyZXFVdWlkIjoiYTZjMGRjNGU0MTZiNDdiM2FmZGIyNTkzOGYxMjk4ODYifQ"
	}
}
```

## Grant customer access to a site

Give a Customer account access to a a specific site. This will allow them to access the site inside their account. Both site and account resources must exist already. You may pass an optional JSON permissions array that gives the customer access only to certain features. If no permissions object is passed, the customer will be given access to all features by default.

When giving customers access to a site, it is important to note that some permissions are dependent on other permissions being present. For example, you cannot give a customer access to a site's SEO if they do not have permission to edit the site first. See below for which permissions are dependent on others for each site type. 

If you would like to update the permissions that a specific customer has to a site, you can call the same API to overwrite any existing permissions. 

### Method and path
`POST /accounts/{account_name}/sites/{site_name}/permissions`

> Array permissions to send

```json
{
"permissions":
	[
		"PUSH_NOTIFICATIONS",
		"REPUBLISH",
		"EDIT",
		"LIMITED_EDITING",
		"INSITE",
		"PUBLISH",
		"CUSTOM_DOMAIN",
		"RESET",
		"E_COMMERCE",
		"SEO",
		"STATS_TAB",
		"DEV_MODE",
		"BACKUPS",
		"BLOG"
	]
}
```

> cURL Example:

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions \
-d '{"permissions":["EDIT","DEV_MODE","STATS_TAB"]}'
```

### Permission options:
Permission Name | Dependency | Description
---------- | ---------- | ----------
STATS_TAB | (None) | Access to the statistics for this website.
EDIT | (None) | Allow the user to perform all possible edits to the website, such as delete elements, move them add new content.
DEV_MODE | EDIT | Allow direct access to the HTML & CSS of the website. This still allows the user to use the HTML embed widget.
INSITE | EDIT | Add, edit, or delete existing website personalization rules. 
E_COMMERCE | (None) | Manage catalogue, view orders and control store settings.
SEO | EDIT | Set SEO settings on the site or page level.
CUSTOM_DOMAIN | EDIT | Set or edit the domain of a website. Can only be accessed on published websites.
BLOG | (None) | Give access to write new posts, edit existing ones and manage blog settings.
REPUBLISH | EDIT | Update the live site with all changes made in the editor.
PUBLISH | REPUBLISH | Publish the site for the first time. *Note: When publishing a site for the first time, the account will be automatically charged.*
BACKUPS | EDIT | Create, restore and delete backups.
RESET | EDIT | Reset a site, will allow the customer to pick a new template for the site as part of resetting.
PUSH_NOTIFICATION | EDIT | Ability to send push notifications to visitors who have subscribed to notifications from the website.
LIMITED_EDITING | (None) | Allow the customer to only edit widget content already in the website. They cannot move, delete or add widgets or change the design. Note that any permission that has the EDIT requirement cannot be used in conjunction with limited editing.

## Get multiscreen site permissions

Get a list of available permissions for a Customer account when granting access to a multiscreen website. This list will be updated in the future as new features are released, so it is best to check regularly for updates.

### Method and path
`GET /accounts/permissions/multiscreen`

> JSON Response:

```json
[
    "PUBLISH",
    "CUSTOM_DOMAIN",
    "DEV_MODE",
    "E_COMMERCE",
    "REPUBLISH",
    "INSITE",
    "SEO",
    "STATS_TAB",
    "RESET",
    "BACKUPS",
    "BLOG",
    "LIMITED_EDITING",
    "EDIT",
    "PUSH_NOTIFICATIONS"
]
```

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/permissions/multiscreen
```

## Get site permissions for a customer

Get the permissions that a customer has been granted for a specific site. Each site that a customer is granted access to can have different permissions, so each association can potentially have different permissions. Permissions can be returned for either mobile or multiscreen websites.  

### Method and path
`GET /accounts/{account_name}/sites/{site_name}/permissions`

### Return
You can expect a `200 OK` response code for a successful call.
 
> JSON Example

```json
{
	"permissions": [
		"STATS_TAB",
		"EDIT",
		"PUBLISH",
		"REPUBLISH",
		"STATS_EMAIL",
		"SEO",
		"CUSTOM_DOMAIN",
		"BACKUPS"
	]
}
```
```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions
```

## Remove customer access to site

Remove/delete access that a customer has to a specific mobile or multiscreen site. 

### Method and path
`DELETE /accounts/{account_name}/sites/{site_name}/permissions`

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X DELETE \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions
```
 
### URI Parameters:

- account_name - An existing Duda account that exists under your account
- site_name - An existing site. Can be a multiscreen or mobile site

### Return:

You can expect a `204 No Content` response code for a successful delete access call.

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X DELETE \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions
```

## Subscribe customer to stats

Sign up customer or staff member for weekly or monthly statistics emails for a specific site. These emails will be automatically delivered by the Duda platform to your users. Monthly emails are sent on the 3rd of each month and weekly emails are sent every Tuesday. You can also perform the same API call to overwrite the existing frequency of customers already signed up.

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email \
-d '{"frequency":"WEEKLY"}'
```

### Method and path
`POST /accounts/{account_name}/sites/{site_name}/stats-email`

### Parameters
- account_name - URL parameter - Account name
- site_name - URL parameter - A reference to a specific site

You can also send a JSON object in the body of the request which tells Duda the frequency to send the customers emails: "MONTHLY" or "YEARLY".

## Get stats email settings
Get the status of stats emails for this customer. If a recurring email is already enabled, a JSON object with the frequency will be returned. If no recurring email is enabled, then an error will be returned.

### Method and path
`GET /accounts/{account_name}/sites/{site_name}/stats-email`

> JSON Response

```json
{
	"frequency":"WEEKLY"
}
```
> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email
```

### Parameters
- account_name - URL parameter - Account name
- site_name - URL parameter - A reference to a specific site

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X DELETE \
-k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email
```

### Response
You can expect a `200 OK` response code for a successful call.

## Unsubscribe customer stats email
Stop a user from receiving recurring stats emails.

### Method and path
`DELETE /accounts/{account_name}/sites/{site_name}/stats-email`

### Parameters
- account_name - URL parameter - Account name
- site_name - URL parameter - A reference to a specific site

### Response
You can expect a `204 No Content` response code for a successful delete call.

## Get multiscreen sites by account

Get all websites that a specific customer account has access to. This is useful for reporting and also listing a dashboard of websites for an end-user to access/edit. 

> JSON Return:

```json
[
	{
		"site_name":"08e5f101"
	},
	{
		"site_name":"a7fa3956"
	},
	{
		"site_name":"u8elal2"
	}
]
```

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/grant-access/john@johnsmith.com/sites/multiscreen​
```
### Method and path

`GET /accounts/grant-access/{account_name}/sites/multiscreen`

### Parameters:

- account_name - URL parameter - Account name

### Return
You can expect a `200 OK` response back with an array of site name objects for this customer.

## Get reset pass link

In order to allow your users to log in directly to the white label dashboard, they must set up a password. Using the Get Reset Password URL you can generate a unique URL to allow your users to access their dashboard/editor directly. We recommend emailing the Reset Password link directly to your customers. This URL is only valid for 30 days. 

> JSON Return

```json
{
	"reset_url" : "http://example.mobilewebsiteserver.com/login/resetpwd?uuid=e4ec7b6f-b77f-47de-aedf-d2383ccb5de2"
}
```

> Example:

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.dudamobile.com/api/accounts/reset-password/johnsmith@gmail.com
```

### Method and path
`POST /accounts/reset-password/{account_name}`

### Return
With a successful call, you should receive a `200 OK` status response. 
