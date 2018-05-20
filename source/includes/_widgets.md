# Custom Widgets

Duda has an API for custom widgets. This relates to widgets that exist within your master account. Currently, this API is used to localize widgets for different languages. Note that you must follow the instructions here to set up widgets for localization. 

Widget localiation work with key value parings. Inside of widget builder, you add a key value combination, where the key is used as a variable and the value is the default fallback for all languages. When localizing widgets, you send Duda a key value pair with the localized content for each specific locale you need to support. 

## Get all custom widgets

> Example:

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/widgets/list
```

> JSON Response

```json
[
  {
    "title": "Card",
    "id": "812a0ed5f2a54ccc8a66ccfd6fab3fc8"
  },
  {
    "title": "FAQ",
    "id": "f3e1efc5fb2e44d185c9a41643d11945"
  },
  {
    "title": "Facebook Page Feed",
    "id": "ab485aff625a480bbb2c660e880b0fbd"
  }
]
```

Return an array of objects that describe all widgets in your account. Duda will return the title (name) and unique identifer for that widget. Note that this only returns custom widgets which are in your account. 

### Method and path
`GET /widgets/list`

### Response

You can expect a `200 OK` Response code for a successful call.

## Scan for available strings

Search a widget for all available localization keys that have been inserted into the widget. This will return a list of all key value parings that have been added to the editors, HTML or JavaScript sections of the widget. The value that is returned will be the default text that was originally entered into the widget.

Duda allows you to install localzion placeholders within three locations of widgets: (1) Content & Design Editors, (2) HTML (3) JavaScript. See below for how they shouild be formatted:

Install Area | Example
---------- | ----------
Content or Design Editor | {{key=default value/text}}
HTML | {{localize 'key' 'DEFAULT TEXT'}}
JavaScript | api.localize('key', 'DEFAULT TEXT')

You can install the placeholders above into any area that displays text to the user directly, including the labels, placeholders, widget, HTML, JS, etc.. This API call will return all possible strings to be localized. 

> Example 

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/widgets/{widgetId}/strings/scan
```

> JSON Response:

```json
{
  "HTMLKey": "DEFAULT TEXT in HTML",
  "key": "DEFAULT TEXT EDITOR",
  "JSKey": "DEFAULT TEXT JS"
}
```

### Method and path
`GET /widgets/{widgetId}/strings/scan`

### Response

You can expect a `200 OK` response code with an object of key value pairs. 

## Get Widget Locale Strings

Get all localized strings for a specific widget for a specific locale. You will pass Duda both the Widget ID and the locale of which you want to see what has already been set for a specific widget.  

Duda will return the key and then the language specific copy as the value to that key. 

> Example 

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/widgets/f3e1efc5fb2e44d185c9a41643d11945/strings/en_gb
```

> JSON Response:

```json
{
    "FAQ_title":"English UK FAQ Title"
}
```

### Method and Path
`GET /widgets/{widgetId}/strings/{localeCode}`

### Parameters
- Widget ID: The unique ID representing the widget, returned originally by the Get All Widget API call. 
- Locale Code: A string representing the of the language you want to return

### Response
You can expect a `200 OK` response code for a successful call with a JSON object that contains the key and localized content. 

## Update widget locale strings

Set the strings of a widget for a specific language. You can pass any key value pair. You will want to pass a JSON object of Key/Value entries that define the StringKey/String that will be used.

You can pass key value pairs that are not yet installed in the widget, because Duda localizes widgets at the time of output/runtime, depending on the locale of the website or widget.

> Example Body to Send for Spanish:

```json
{
    "Widget.FAQ.Label":"FAQ Artículos",
    "Widget.FAQ.Tooltip":"Introduzca cada título, descripción, etc.",
    "Widget.FAQ.AddItem.Text":"Añadir nueva pregunta"
}
```

>Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.duda.co/api/widgets/f3e1efc5fb2e44d185c9a41643d11945/strings/es
-d '{
    "Widget.FAQ.Label":"FAQ Artículos",
    "Widget.FAQ.Tooltip":"Introduzca cada título, descripción, etc.",
    "Widget.FAQ.Add.Item.Text":"Añadir nueva pregunta"
}'
```

### Method and path

`POST /widgets/{widgetId}/strings/{localeCode}`

### Parameters

URI Parameters:

- Widget ID: The unique ID representing the widget, returned originally by the Get All Widget API call. 
- Locale Code: A string representing the of the language you want to return

Body data:

A JSON object of key value pairs. 

### Response

You can expect a `204 No Content` response for a successful call. 

## Publish widget locale strings

Publish all the custom locale strings that have been added to the widget. This will only make the newly added strings available to live widgets. This only publishes the strings -- not the entire widget.

<aside class="warning">The placeholders within the widget editors, HTML and JavaScript will need to be installed in the widget and published before you will see new strings appear.</aside>

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X POST \
-k https://api.duda.co/api/widgets/f3e1efc5fb2e44d185c9a41643d11945/strings/publish
```

### Method and path
`POST /widgets/{widgetId}/strings/publish`

### Response
A successful call will return a `204 No Content` HTTP response code.

## Get available locales
Get a list of available locales that the widget can be localized for. We will return an array of [language](#langauges) codes.

> Example

```shell
curl -S -u 'APIusername:APIpassword' \
-H 'Content-Type: application/json' \
-X GET \
-k https://api.duda.co/api/widgets/locales
```

> JSON Response:

```json
[
    "en",
    "es",
    "pt",
    "fr",
    "de",
    "tr",
    "en_gb",
    "it",
    "nl",
    "es_ar"
]
```

### Method and path
GET /widgets/locales