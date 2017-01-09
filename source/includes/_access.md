## Access

To access the Duda API, you need to request access through your websites dashboard. This can be found under the 'Account tab' at the top of the dashboard navigation. Once you request access, Duda will need to approve the request, which takes upto 48 hours. You will only receive your password via email. Please keep the API Username and Password safe, as anyone with this information can access your account. 

The Duda API is free for anyone to access and use. You should take note of the payments section as we currently do not offer free ad-supported sites created via the API.

## Endpoints

Duda's primary endpoint for all production work is: 

`https://api.dudamobile.com/api`

The documentation here assumes that you will combine the endpoint with the path of the API method. There are some cases where you might have a different endpoint than the one above, such as working with Duda's sandbox environment or a custom setup that Duda provides, although this is rare. 

## Schema

All data passed to and from the Duda API will be in JSON representation. We currently only support this format of data transfer.

## Dates

There are some Duda APIs that return date time values. Duda formates these in standard universal full date/time pattern in the UTC timezone. The format is: yyyy-mm-ddThh:mm:ss. So for example: 2016-11-15T22:55:53 would be November 11th, 2016 at 10:55:53 PM UTC.

When sending dates to Duda, you can use the full universal format above, or just the YYYY-MM-DD format. If you omit the time -- DUda will default that time to the beginning of the day. 

## Langauges

There are some parts of the API where Duda returns real world content to display to users. As part of this, we are able to deliver localized content. Below you will see a full list of languages that the Duda platform (UI) is localized for along with their corresponding language codes. 


Language | Language Code
--------- | -------
English | en
