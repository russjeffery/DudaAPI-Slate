## Authentication

```shell
curl -X GET -i 'https://api.dudamobile.com/api/sites/multiscreen/templates' \
	-H 'Authorization: ZXhhbXBsZVVzZXI6YmUkdHBAc3M=' \ 
	-H 'Content-Type: application/json'
```
> Instead of explicitly setting the Authorization header, you can use the -u 'APIUser:APIPass' command with cURL instead.

The Duda API uses <a href="https://en.wikipedia.org/wiki/Basic_access_authentication" target="_blank">basic HTTP authentication</a> to authenticate requests and access resources within your account. As part of getting access to the API, you will get a user name and password combination that is specific to your account. This information should be passed as part of every API request you make to the platform. It is also required that you send this in a preemptive fashion. 

To create the HTTP Authorization Header, you should combine your username and password into one string, separated by a colon and then Base64 encode that string. So for example, 'exampleUser:be$tp@ss' combination would result in the header:

`Authorization: ZXhhbXBsZVVzZXI6YmUkdHBAc3M=`

<aside class="notice">
Make sure to set the authorization header with your unique API username and API password on each request.
</aside>

## Security 

The Duda REST API is available over HTTPS only to ensure data privacy. Unencrypted HTTP is not supported. As of the current version, the Duda REST API is secured with HTTP Basic Authentication only.

Duda recommends requiring a TLS connection instead of SSL. The Duda API does not accept SSLv3 connections, due to the [POODLE venerability](http://en.wikipedia.org/wiki/POODLE).  