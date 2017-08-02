# Analytics

## Get Analytics

Get analytics history for a specific website over a certain amount of time.

### Method and path
`GET /analytics/site/{site_name}`

### Parameters
The following URL query parameters can be added on the end of the GET site API call.

Analytics are updated every 24 hours, so you will not be able to pull analytics from the current day, only previous days.

Query String Parameter | Type | Required | Description
---------- | ---------- | ---------- | ----------
from | Date | Optional. Defaults to 30 days prior to "to" value | Give the start (date)[#date] of when you want to pull analytics from.
to | Date | Optional. Defaults to the current day. | Give an end date.
dimension | String | Optional | The type of dimension to query the data by: (1) "system": Set including Browser Type, Browser Version and Operating System, or (2) "geo": Country (and Region where available)
result | String | Optional. Either "traffic" or "activities". Defaults to traffic. | Whether to return results with **traffic** metrics (visits, unique visitors, pageviews), or to return results with **activities** metrics (click_to_calls, click_to_maps, click_to_emails, form_submits)
dateGranularity | String | optional, can be: "DAYS", "WEEKS", "MONTHS", or "YEARS" | When the dateGranularity URL parameter is present, Duda will return a data from the selected format. If you choose days for a specific week, Duda will return the visits for that specific day and each day within the date range defined. 

If neither the "from" parameter nor the "to" parameter is provided, the queried period will be the past 30 days. 

If only the "from" parameter is provided, the queried period will be the "from" date until today.

If only the "to" parameter is provided, the queried period will be from 30 days prior to the "to" date until the "to" date.

<aside class="warning">The ?results=activities cannot be combined with the 'dimension' query string. This will return an error.</aside>

> Querying: Traffic, not dimensioned:

```json
{"UNIQUE_VISITORS":50,"VISITS":100,"PAGE_VIEWS":150}
```

> Querying: Activities, not dimensioned:

```json
{"CLICK_TO_EMAILS":2,"FORM_SUBMITS":5, "CLICK_TO_MAPS":15,"CLICK_TO_CALLS":20}
```

> Querying: Traffic, Dimensioned by geo:

```json
[ {
    "data" : {
        "VISITORS" : 50,
        "PAGE_VIEWS" : 150,
        "VISITS" : 100
    },
    "dimension" : {
        "country" : "US",
        "region" : "CA"
    }
}, {
    "data" : {
        "VISITORS" : 50,
        "PAGE_VIEWS" : 150,
        "VISITS" : 100
    },
    "dimension" : {
        "country" : "US",
        "region" : "FL"
    }
}, {
    "data" : {
        "VISITORS" : 50,
        "PAGE_VIEWS" : 150,
        "VISITS" : 100
    },
    "dimension" : {
        "country" : "Canada",
        "region" : "Ontario"
    }
} ]
```

### Return
**If the result parameter is "traffic":**

If a dimension set is provided, an array containing the following JSON elements for each row of the result set.

If no dimension set is provided, a JSON structure containing the following elements.

Parameter | Type | Description
---------- | ---------- | ----------
dimension | JSON Structure | A single dimension pertaining to the queried dimension type (if a dimension type was sent). Ex: The "geo" dimension type may return "dimension":{"Country":"Canada","Region":"Ontario"}
VISITS | integer | For the queried period and for the specified dimension, the number of visits to the site.
VISITORS | integer | For the queried period and for the specified dimension, the number of unique visitors to the site.
PAGE_VIEWS | integer | For the queried period and for the specified dimension, the number of page views to the site.

**If the result parameter is "activities":**

If a dimension set is provided, an array containing the following JSON elements for each row of the result set.

If no dimension set is provided, a JSON structure containing the following elements.

Parameter | Type | Description
---------- | ---------- | ----------
dimension | JSON Object | A single dimension pertaining to the queried dimension type (if a dimension type was sent). Ex: The "geo" dimension type may return "dimension":{"Country":"Canada","Region":"Ontario"}
click_to_calls | integer | For the queried period and for the specified dimension, the number of click-to-call actions on the site.
click_to_maps | integer | For the queried period and for the specified dimension, the number of click-to-map actions on the site.
click_to_emails | integer | For the queried period and for the specified dimension, the number of click-to-email actions on the site.
form_submits | integer | For the queried period and for the specified dimension, the number of form submission actions on the site.

