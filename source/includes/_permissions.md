# Permission Groups

Permission groups are a resource of a DudaPro account. Groups set the exact features that Staff members will have access to. Each group has a set of Permissions that defines the exact features a Staff member can access while logged into their account. Staff accounts must be assigned to a group in order to access the platform and can only be assigned to one group at a time.

## Get default groups 

As part of the Permission Groups, Duda provides a set of default (predefined) groups that cannot be edited. These groups are set by Duda and are available for everyone to use. The big advantage of using the default groups is that they will be updated in the future as new features are released by Duda. If you create your own custom groups, Duda will not automatically add the feature to your custom group.

> JSON Response

```json
[{
    "group_name": "administrator",
    "color": "rgb(253,113,34)",
    "title": "Admin",
    "permissions": ["EDIT", "CREATE", "DELETE", "API", "DUDA_PRO", "MANAGE_USERS", "STATS", "E_COMMERCE", "MARKETING", "REPUBLISH", "PUBLISH", "DEV_MODE", "PURCHASE_IMAGES", "SITE_CUSTOM_DOMAIN"]
}, {
    "group_name": "salesman",
    "color": "rgb(36,206,151)",
    "title": "Sales",
    "permissions": ["CREATE", "MARKETING", "STATS", "REPUBLISH", "EDIT", "E_COMMERCE"]
}, {
    "group_name": "designer",
    "color": "rgb(21,193,191)",
    "title": "Designer",
    "permissions": ["CREATE", "MARKETING", "REPUBLISH", "EDIT", "E_COMMERCE"]
}, {
    "group_name": "storemanager",
    "color": "rgb(53,188,221)",
    "title": "Store Manager",
    "permissions": ["E_COMMERCE"]
}]
```
> Example:

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/permission-groups/default
```

### Method and path
`GET /permission-groups/default`

### Response

You can expect a `200 OK` Response code for a successful call.

## Create custom group

Create a new staff group. This group defines the features that a staff member will have access to when logging into their account. When creating a custom group, Duda will return a unique group_name to the Duda platform. We recommends storing the group_name value for future reference.

> Example JSON to send:

```json
{
    "color": "rgb(27,245,71)",
    "title": "Outside Sales",
    "permissions": ["STATS", "CREATE", "REPUBLISH", "EDIT"]
}
```
> Example 

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.duda.co/api/permission-groups/create \
-d '{"group_name": "Mzc0MQ","color": "rgb(27,245,71)","title": "Outside Sales","permissions":["STATS", "CREATE", "REPUBLISH", "EDIT"]}' 
```

> JSON Response:

```json
{
    "group_name":"MjAwMg"
}
```

### Method and Path
`POST /permission-groups/create`

### Parameters

You will need to send a JSON object that describes the group you wish to create:

Property | Type | Required | Description
---------- | ---------- | ---------- | ----------
title | String | Required | A short title to describe the group being created. Should be descriptive.
color | RGB/HEX color string | Optional | A hex or RGB color code to associate the group with. For example, #2d6598 or rgb(45,101,152).
permissions | array | Required | An array of strings that set the permissions for this group. 

### Return
You can expect a `200 OK` response code for a successful call with a JSON object containing a unique group_name.

## Get custom groups

Get all custom groups that have been created for this account. These are groups which have been created through the API or within the Duda Users &amp; Permissions section of the dashboard. 

> JSON Response:

```json
[{
    "group_name": "Mzc0MQ",
    "color": "rgb(27,245,71)",
    "title": "Outside Sales",
    "permissions": ["STATS", "CREATE", "REPUBLISH", "EDIT"]
}, {
    "group_name": "MzczNQ",
    "color": "rgb(45,101,152)",
    "title": "QA",
    "permissions": ["PUBLISH", "REPUBLISH", "EDIT"]
}]
```

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/permission-groups/custom
```

### Method and path
`GET /permission-groups/custom`

### Response
You can expect a `200 OK` Response code for a successful call with an array of objects describing the custom group. You can see the details about each value returned in the create custom group section.

## Update custom group

Update an existing group. Use this if you want to change the features that a certain staff group has access to, change the title (name) of the group or set a new color to apply to the group.

> Example JSON to send:

```json
{
  "title": "Outside Sales",
  "color": "#85144b",
  "permissions": [
    "PRO_SETTINGS",
    "STATS",
    "MANAGE_STAFF",
    "CREATE_SITES",
    "DELETE_SITES",
    "MARKETING",
    "E_COMMERCE",
    "PUBLISH",
    "DEV_MODE",
    "EDIT_SITES",
    "CUSTOM_DOMAIN",
    "REPUBLISH"
  ]
}
```

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.duda.co/api/permission-groups/Mzc0MQ/update \
-d '{"color": "rgb(27,245,71)","title": "Outside Sales","permissions":["STATS", "CREATE", "REPUBLISH", "EDIT","CREATE_SITES"]}'
```

### Method and path

`POST /permission-groups/{group_name}/update`

### Return 
You can expect a `204 No Content` HTTP response code upon successful call.

## Get staff group permissions

Get the details of a specific permission group.

> Example:

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET -k https://api.duda.co/api/permission-groups/Mzc0MQ
```

> Example response body:

```json
{
  "title": "Outside Sales",
  "color": "#85144b",
  "permissions": [
    "PRO_SETTINGS",
    "STATS",
    "MANAGE_STAFF",
    "CREATE_SITES",
    "DELETE_SITES",
    "MARKETING",
    "E_COMMERCE",
    "PUBLISH",
    "DEV_MODE",
    "EDIT_SITES",
    "CUSTOM_DOMAIN",
    "REPUBLISH"
  ]
}
```

### Method and path

`GET /permission-groups/{group_name}`

### Parameters

- group_name - A unique ID that relates back to this specific group.

### Response

You can expect a `200 OK` HTTP response for a successful call. 

## Delete custom group

Delete an already created custom group. This can't be undone. 

<aside class="warning">You can't delete a group with any staff accounts in it. You must first assign these staff accounts to other groups or delete them.</aside>

### Method and path

`DELETE /permission-groups/{group_name}`

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X DELETE -k https://api.duda.co/api/permission-groups/Mzc0MQ 
```

### Parameters

- group_name - The uniquie name of the group. 

### Return

You can expect a `204 No Content` HTTP response code for successful delete calls.

## Assign staff to group

Assign a Staff Account to a specific group. Both the custom group and staff account must exist already. The account must be defined as a staff account. If this Staff acconut is already assigned to a group, they will be reassigned to the new group instead.

> Example call

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST -k https://api.duda.co/api/permission-groups/Mzc0MQ/accounts/john-smith@gmail.com/add
```

### Method and path

`POST /permission-groups/{group_name}/accounts/{account_name}/add`

### Parameters

- group_name - The uniquie name of the group. 
- account_name - A unique account name that was set while originally creating the account (usually an email address)

### Return

You can expect a `204 No Content` HTTP response code for successful assign calls.

## Remove staff from group

Remove a staff account from the group that they are currently assigned to. Please note that this will prevent the staff user from logging in, as staff account must be assigned to a group to log in.

> Example Call

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST -k https://api.duda.co/api/permission-groups/Mzc0MQ/accounts/john-smith@gmail.com/add
```

### Method and path
`DELETE /permission-groups/{group_name}/accounts/{account_name}`

### Parameters

- group_name - The uniquie name of the group. 
- account_name - A unique account name that was set while originally creating the account (usually an email address)


