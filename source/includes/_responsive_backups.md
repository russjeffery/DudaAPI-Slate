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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/backups/b4ra2g' \
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
curl -X GET -k 'https://api.duda.co/api/sites/multiscreen/backups/b4ra2g' \
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
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/backups/b4ra2g/restore/QA-Complete' \
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
curl -X DELETE -k 'https://api.duda.co/api/sites/multiscreen/backups/b4ra2g/restore/QA-Complete' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

### Return

Upon success, Duda will return a `204 No Content` HTTP status code.
