# Permission Groups

Permission groups are a resource of a DudaPro account. Groups set the exact features that Staff members will have access to. Each group has a set of Permissions that defines the exact features a Staff member can access while logged into their account. Staff accounts must be assigned to a group in order to access the platform and can only be assigned to one group at a time.

## Get Default Groups 

As part of the Permission Groups, Duda provides a set of default (predefined) groups that cannot be edited. These groups are set by Duda and are available for everyone to use. The big advantage of using the default groups is that they will be updated in the future as new features are released by Duda. If you create your own custom groups, Duda will not automatically add the feature to your custom group.

> JSON Response

```json
[{
    "group_name": "administrator",
    "color": "rgb(253,113,34)",
    "title": "Admin",
    "permissions": ["EDIT", "CREATE", "DELETE", "API", "DUDA_PRO", "MANAGE_USERS", "STATS", "E_COMMERCE", "MARKETING", "REPUBLISH", "PUBLISH", "DEV_MODE", "SITE_CUSTOM_DOMAIN"]
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
-k https://api.dudamobile.com/api/permission-groups/default
```

### Method and path
`GET /permission-groups/default`

### Response

You can expect a `200 OK` Response code for a successful call.

## Get Custom Groups

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
-k https://api.dudamobile.com/api/permission-groups/custom
```

### Method and path
`GET /permission-groups/custom`

### 

