# Account

An account resource represents a single Staff or Customer account. There are three account types referenced below, master account, Staff account and Customer account. The master account is the Duda account that accesses the Duda API.

## Create Account

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

> cURL Example

```shell curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' \
-X POST -i -k https://api.dudamobile.com/api/accounts/create \
-d '{"account_name":"email@email.com", "first_name":"john", "last_name":"doe"}'
```

## Get Account 

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
## Delete Account

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
## Update Account

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

## Generate Single Sign-on Token

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
## Generate SSO Link
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
Dashboard | (none) | (none) | Send the user directly into their website dashboard. You do not need to send a target or site_name parameter.
Stats | "TARGET" | "STATS" | Send the user directly into the website statistics URL. 
Site Editor | "TARGET" | "EDITOR" | Send the user directly into the white labeled website builder.
Stats | "TARGET" | "RESET_SITE" | Send the user directly to the select new template / reset site page. Note that if the user does reset the site, all existing website edits will be lost and the site will be reset to a new template that is selected. 

## Grant Customer Access to a Site

Give a Customer account access to a a specific site. This will allow them to access the site inside their account. Both site and account resources must exist already. You may pass an optional JSON permissions array that gives the customer access only to certain features. If no permissions object is passed, the customer will be given access to all features by default.

When giving customers access to a site, it is important to note that some permissions are dependent on other permissions being present. For example, you cannot give a customer access to a site's SEO if they do not have permission to edit the site first. See below for which permissions are dependent on others for each site type. 

If you would like to update the permissions that a specific customer has to a site, you can call the same API to overwrite any existing permissions. 

### Method and path
`POST /accounts/{account_name}/sites/{site_name}/permissions`

> Array permissions to send
```json
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
```
> cURL Example:
```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.dudamobile.comhttps://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions \
-d '{"permissions":["EDIT","DEV_MODE","STATS_TAB"]}'
```

### Permission options:
Permission Name | Dependency | Description
---------- | ---------- | ----------
STATS_TAB | (None) | Access to the statistics for this website.
EDIT | (None) | Allow the user to perform all edits to the website, such as delete elements, move them add new ones and mre.
DEV_MODE | EDIT | Allow direct access to the HTML & CSS of the website. This still allows the user to use the HTML embed widget.
INSITE | EDIT | Add, edit, or delete existing website personalization rules. 
E_COMMERCE | (None) | Manage catalogue, view orders and control store settings.
SEO | EDIT | Set SEO settings on the site or page level.
CUSTOM_DOMAIN | EDIT | Set or edit the domain of a website. Can only be accessed for published websites.
BLOG | (None) | Give access to write new posts, edit existing ones and manage blog settings.
REPUBLISH | EDIT | Update the live site with all changes made in the editor.
PUBLISH | REPUBLISH | Publish the site for the first time. *Note: When publishing a site for the first time, the account will be automatically charged.*
BACKUPS | EDIT | Create, restore and delete backups.
RESET | EDIT | Reset a site, will allow the customer to pick a new template for the site as part of resetting.
PUSH_NOTIFICATION | EDIT | Ability to send push notifications to visitors who have subscribed to notifications on the website.
LIMITED_EDITING | (None) | Allow the customer to only edit widget content already in the site. They cannot move, delete or add widgets or change the design. Note that any permission that has the EDIT requirement cannot be used in conjunction with limited editing.

## Get Available Multiscreen Site Permissions

Get a list of available permissions for a Customer account when granting access to a multiscreen website. This list will be updated in the future as new features are released, so it is best to check regularly for updates.

### Method and path
`GET /accounts/permissions/multiscreen`

> JSON Response:

```json
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


```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.dudamobile.com/api/accounts/permissions/multiscreen
```


</table>
 <strong>Mobile Permissions:</strong> 
<table border="1" cellspacing="1" cellpadding="1">
<thead>
<tr>
<th>Permission Name</th>
<th>Dependency</th>
<th>Description of Permission</th>
</tr>
</thead>
<tbody>
<tr>
<td>STATS_TAB</td>
<td><em>(None)</em></td>
<td>Access to statistics for this  site. </td>
</tr>
<tr>
<td>EDIT</td>
<td><em>(None)</em></td>
<td>Add elements, manage pages, edit design, control site settings.</td>
</tr>
<tr>
<td>HTML_CSS</td>
<td>EDIT</td>
<td>Directly access HTML/CSS of the site.</td>
</tr>
<tr>
<td>SEO</td>
<td>EDIT</td>
<td>Configure SEO settings on a site or page level.</td>
</tr>
<tr>
<td>CUSTOM_DOMAIN</td>
<td>EDIT</td>
<td>Edit the site's domain.</td>
</tr>
<tr>
<td>REPUBLISH</td>
<td>EDIT</td>
<td>Update the live site with all changes made in the editor.</td>
</tr>
<tr>
<td>PUBLISH</td>
<td>REPUBLISH</td>
<td>Publish the site for the first time. <em>Note: When publishing a site for the first time, the account will be automatically charged.</em></td>
</tr>
<tr>
<td>BACKUPS</td>
<td>EDIT</td>
<td>Create, restore and delete backups.</td>
</tr>
</tbody>
</table>
<div class="api-code">POST /accounts/{account_name}/sites/{site_name}/permissions</div>
 <strong>URI Parameters:</strong> 
<ul>
<li>account_name - An existing Duda account that exists under your account</li>
<li>site_name - An existing site. Can be a multiscreen or mobile site</li>
</ul>
 <strong>Data structure (body)</strong> 
 If you have specific permissions you want this customer to have access to, pass a permissions object with an array of specific permission strings. 
 <em>JSON example:</em> 
<pre>{
          "permissions": ["EDIT", "DEV_MODE", "STATS_TAB"]
      }</pre>
 <strong>Expected Return:</strong> 
 Http Status Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">AccessForbidden</div>
<div class="api-error">ResourceNotExist</div>
<div class="api-error">ResourceAlreadyExist</div>
<div class="api-error">InvalidInput</div>
 <strong>CURL Example:</strong> 
<pre><span class="nowiki">curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X POST -i -k https://api.dudamobile.comhttps://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions -d '{"permissions":["EDIT","DEV_MODE","STATS_TAB"]}'</span></pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="get-access-permissions">
<h3 id="get-access-permissions">Get Site Permissions for a Customer</h3>
<div class="hidden-content" style="">
 Get the permissions that a customer has been granted for a specific site. Each site that a customer is granted access to can have different permissions, so each association can potentially have different permissions. Permissions can be returned for either mobile or multiscreen websites.  
<div class="api-code">GET /accounts/{account_name}/sites/{site_name}/permissions</div>
 <strong>Expected Return:</strong> 
 Http Status Code: 200 
 <em>JSON:</em> 
<pre><span class="sBrace structure-1">{
      "permissions":["STATS_TAB","EDIT","PUBLISH","REPUBLISH","STATS_EMAIL","SEO","CUSTOM_DOMAIN","BACKUPS"]
      }
      </span></pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">AccessForbidden</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="get-permissions">
<h3 id="get-permissions">Get Customer Site Access</h3>
<div class="hidden-content" style="">
 See which permissions a customer has access to for a specific site. Can be a mobile or multiscreensite. 
<div class="api-code">GET /accounts/{account_name}/sites/{site_name}/permissions</div>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 200 OK 
 <em>JSON:</em> 
<pre>{
          "permissions": ["EDIT", "DEV_MODE", "STATS_TAB"]
      }
      </pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">AccessForbidden</div>
<div class="api-error">ResourceNotExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="delete-permissions">
<h3 id="delete-permissions">Remove Customer Access to Site</h3>
<div class="hidden-content" style="">
 Remove/delete access that a customer has to a specific mobile or multiscreen site. 
<div class="api-code">DELETE /accounts/{account_name}/sites/{site_name}/permissions</div>
 <strong>URI Parameters:</strong> 
<ul>
<li>account_name - An existing Duda account that exists under your account</li>
<li>site_name - An existing site. Can be a multiscreen or mobile site</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">AccessForbidden</div>
<div class="api-error">ResourceNotExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X DELETE -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/permissions</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="get-multiscreen">
<h3 id="get-multiscreen">Get Multiscreen Sites by Account</h3>
<div class="hidden-content" style="">
 Get a list of all multiscreen site_name(s) that have been granted access to a certain Sub-account. This is often used for building a list of sites to be used as a dashboard or for listing all sites the user might have access to.  
<div class="api-code">GET /accounts/grant-access/{account_name}/sites/multiscreen</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Code: 200 No Content<br>JSON Array: 
<pre><span id="s-1" class="sBracket structure-1">[</span>
      <span id="s-2" class="sBrace structure-2">{</span>
      <span id="s-3" class="sObjectK">"site_name"</span><span id="s-4" class="sColon">:</span><span id="s-5" class="sObjectV">"08e5f101"</span>
      <span id="s-6" class="sBrace structure-2">}</span><span id="s-7" class="sComma">,</span>
      <span id="s-8" class="sBrace structure-2">{</span>
      <span id="s-9" class="sObjectK">"site_name"</span><span id="s-10" class="sColon">:</span><span id="s-11" class="sObjectV">"a7fa3956"</span>
      <span id="s-12" class="sBrace structure-2">}</span><span id="s-13" class="sComma">,</span>
      <span id="s-14" class="sBrace structure-2">{</span>
      <span id="s-15" class="sObjectK">"site_name"</span><span id="s-16" class="sColon">:</span><span id="s-17" class="sObjectV">"u8elal2"</span>
      <span id="s-18" class="sBrace structure-2">}</span>
      <span id="s-19" class="sBracket structure-1">]</span></pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/grant-access/john@johnsmith.com/sites/multiscreen​</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="get-mobile">
<h3 id="get-mobile">Get Mobile Sites by Account</h3>
<div class="hidden-content" style="">
 Get a list of all mobile site_name(s) that have been granted access to a certain Sub-account. This is often used for building a list of sites to be used as a dashboard or for listing all sites the user might have access to.  
<div class="api-code">GET /accounts/grant-access/{account_name}/sites/mobile</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Code: 200 No Content<br>JSON Array: 
<pre><span id="s-1" class="sBracket structure-1">[</span>
          <span class="sBrace structure-2">{</span>
          <span class="sObjectK">"site_name"</span><span id="s-4" class="sColon">:</span><span id="s-5" class="sObjectV">"tand381"</span>
          <span class="sBrace structure-2">}</span><span id="s-7" class="sComma">,</span>
          <span class="sBrace structure-2">{</span>
          <span class="sObjectK">"site_name"</span><span id="s-10" class="sColon">:</span><span id="s-11" class="sObjectV">"domenicosdeli12"</span>
          <span class="sBrace structure-2">}</span><span id="s-13" class="sComma">,</span>
          <span class="sBrace structure-2">{</span>
          <span class="sObjectK">"site_name"</span><span id="s-16" class="sColon">:</span><span id="s-17" class="sObjectV">"techcrunch81"</span>
          <span class="sBrace structure-2">}</span>
      <span id="s-19" class="sBracket structure-1">]</span></pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/grant-access/john@johnsmith.com/sites/mobile​</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="reset-password">
<h3 id="reset-password">Get Reset Password URL</h3>
<div class="hidden-content" style="">
 In order to allow your users to login directly to the dashboard, they must set up a password. Using the Get Reset Password URL you can generate a URL to allow your users to access their dashboard/editor directly. We recommend emailing the Reset Password link directly to your customers. 
<div class="api-code">POST <a class="link-mailto" title="mailto:/accounts/reset-password/johnsmith@gmail.com" href="/hc/admin/articles/mailto:/accounts/reset-password/johnsmith@gmail.com" rel="freeklink">/accounts/reset-password/johnsmith@gmail.com</a></div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
</ul>
 <strong>Expected Return:</strong> 
 Http Code: 200 
 <em>JSON :</em> 
<pre><span id="s-1" class="sBrace structure-1">{</span>
      <span class="nowiki"><span id="s-2" class="sObjectK">"reset_url" </span><span id="s-3" class="sColon">: </span><span id="s-4" class="sObjectV">"http://example.mobilewebsiteserver.com/login/resetpwd?uuid=e4ec7b6f-b77f-47de-aedf-d2383ccb5de2"</span></span>
      <span id="s-5" class="sBrace structure-1">}</span></pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X POST -i -k https://api.dudamobile.com/api/accounts/reset-password/johnsmith@gmail.com</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="stats-email-create">
<h3 id="stats-email-create">Sign up a Customer or Staff for Recurring Stats Emails</h3>
<div class="hidden-content" style="">
 Sign up customer or staff member for weekly or monthly statistics emails for a specific site. These emails will be automatically delivered by the Duda platform to your users. Monthly emails are sent on the 3rd of each month and weekly emails are sent every Tuesday. 
<div class="api-code">POST /accounts/{account_name}/sites/{site_name}/stats-email</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - A reference to a specific site</li>
</ul>
 <strong>Data structure (body)</strong> 
 Send a JSON frequency object with a string defining frequency. Can be 'MONTHLY' or 'WEEKLY' 
 <em>JSON:</em> 
<pre>{
          "frequency":"WEEKLY"
      }
      </pre>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
<div class="api-error">AccessForbidden</div>
<div class="api-error">ResourceAlreadyExist</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X POST -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email -d '{"frequency":"WEEKLY"}'</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="stats-email-get">
<h3 id="stats-email-get">Get Stats Email Settings</h3>
<div class="hidden-content" style="">
 Get the status of stats emails for this customer. If a recurring email is already enabled, a JSON object with the frequency will be returned. If no recurring email is enabled, then an error will be returned. 
<div class="api-code">GET /accounts/{account_name}/sites/{site_name}/stats-email</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - A reference to a specific site</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 200 OK 
 <em>JSON Returned:</em> 
 <em>JSON:</em> 
<pre>{
          "frequency":"WEEKLY"
      }
      </pre>
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
<div class="api-error">AccessForbidden</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X GET -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="stats-email-update">
<h3 id="stats-email-update">Update Stats Email Settings</h3>
<div class="hidden-content" style="">
 Update the frequency of how often stats emails will be sent to this customer for this site. 
<div class="api-code">POST /accounts/{account_name}/sites/{site_name}/stats-email</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - A reference to a specific site</li>
</ul>
 <strong>Data structure (body)</strong> 
 <em>JSON:</em> 
<pre>{
          "frequency":"MONTHLY"
      }
      </pre>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
<div class="api-error">AccessForbidden</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X POST -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email -d '{"frequency":"MONTHLY"}'</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="stats-email-delete">
<h3 id="stats-email-delete">Delete recurring stats emails</h3>
<div class="hidden-content" style="">
 Stop a user from receiving recurring stats emails. 
<div class="api-code">DELETE /accounts/{account_name}/sites/{site_name}/stats-email</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - A reference to a specific site</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Status Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
<div class="api-error">AccessForbidden</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X DELETE -i -k https://api.dudamobile.com/api/accounts/johnsmith@gmail.com/sites/146856ab/stats-email</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="grant-access">
<h3 id="grant-access">Grant Account with Access Permission to a Site</h3>
<div class="hidden-content" style="">
 <strong>**NOTE: This API is depreciated. Please use the newer <a href="#grant-access-permissions">Grant Customer Access to Site</a> API instead.</strong> 
 You will use this to grant site access to a sub-user account. Both the site and account must be created before granting end user access. As soon as you grant access, the end-user will have access to site in their dashboard or by logging them in with SSO. You can give multiple accounts access to one site or one account access to multiple sites. 
<div class="api-code">POST /accounts/grant-access/{account_name}/sites/{site_name}</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - Site name</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Code: 204 No Content 
 <strong>Possible Errors:</strong> 
<div class="api-error">ResourceNotExist</div>
<div class="api-error">InvalidInput</div>
 <strong>CURL Example:</strong> 
<pre>curl -S -u 'APIusername:APIpassword' -H 'Content-Type: application/json' -X POST -i -k https://api.dudamobile.com/api/accounts/grant-access/johnsmith@gmail.com/sites/1aadcb7c</pre>
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
<li class="revoke-access">
<h3 id="revoke-access">Revoke Account access permission to a Site</h3>
<div class="hidden-content" style="">
 <strong>**NOTE: This API is depreciated. Please use the newer <a href="#delete-permissions">Remove Customer Access to Site</a> API instead.</strong> 
 You will use this in order to remove access to a site for a certain account. For example, if your customer does not pay you, you might want to revoke their access until they are actively paying you again. 
<div class="api-code">POST /accounts/revoke-access/{account_name}/sites/{siteName}</div>
 <strong>Required Parameters:</strong> 
<ul>
<li>account_name - URL parameter - Account name</li>
<li>site_name - URL parameter - Site name</li>
</ul>
 <strong>Expected Return:</strong> 
 HTTP Code: 204 No Content 
<a class="backToTopLink" href="#">Back to Top</a></div>
</li>
</ul>
<script type="text/javascript">// <![CDATA[

// ]]></script>
                  </div>