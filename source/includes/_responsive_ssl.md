## Generate SSL Certificate

Generate a SSL certificate for a specific website. This will enable a HTTPS connection between site visitors and the Duda platform. It usually takes 15-30 minutes to successfully generate a SSL certificate for a website. You can get the status of the SSL certificate by calling the [GET site API](#get-site) and checking the certificate_status parameter. Once the certificate is successfully generated, all website visitors will be redirected to the HTTPS connection. This can be disabled on by updating the site force_https value to false. You do not need to worry about renewing the certificate, as Duda will do this automatically.

### Method and path

`POST /sites/multiscreen/{site_name}/certificate`

URI Parameter:
- site_name

### Return

Upon success, Duda will return a `204 No Content` HTTP code.

```shell
curl -X POST -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/certificate' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```

<aside class="warning">Before sending this API call, it's important that the domain is successfully pointed to Duda's servers. We only currently offer Domain Verified (DV) certificates. As part of the API call, Duda will verify that the domain is correctly pointing at the platform. If it is not, Duda will return an error.</aside>


## Delete SSL Certificate

Delete a certificate that has been generated for a website. This will ensure that the website is served over only an HTTP (insecure) connection and will delete the generated certificate.

### Method and path

`DELETE /sites/multiscreen/{site_name}/certificate`

URI Parameter:
- site_name

### Return

Upon success, Duda will return a `204 No Content` HTTP code.

```shell
curl -X DELETE -k 'https://api.duda.co/api/sites/multiscreen/b4ra2g/certificate' \
	-u 'APIusername:APIpassword' \ 
	-H 'Content-Type: application/json'
```