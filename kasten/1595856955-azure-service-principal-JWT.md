# 1595856955 azure-service-principal-JWT
After creating a Service Principal within azure you have the ability to use this Service Principal and authenticate to an endpoint to recieve an access token. This access token is a valid JWT that contains some meta data that can be used to identity a specific app/SP.



In this example the service principal is using the Username & Password authentication.

For my SP to retrieve an access token the following HTTP request needs to be made:
```bash
curl -X GET -H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=client_credentials&client_id=<app/client ID of SP>&resource=2ff814a6-3304-4ab8-85cb-cd0e6f879c1d&client_secret=<redacted>' \
https://login.microsoftonline.com/<tenant ID>/oauth2/token
```

After making the request above the following body should be returned:
```json
{
  "token_type": "Bearer",
  "expires_in": "3599",
  "ext_expires_in": "3599",
  "expires_on": "1595616849",
  "not_before": "1595612949",
  "resource": "2ff814a6-3304-4ab8-85cb-cd0e6f879c1d",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imh1Tjk1SXZQZmVocTM0R3pCRFoxR1hHaXJuTSIsImtpZCI6Imh1Tjk1SXZQZmVocTM0R3pCRFoxR1hHaXJuTSJ9.eyJhdWQiOiIyZmY4MTRhNi0zMzA0LTRhYjgtODVjYi1jZDBlNmY4NzljMWQiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NTA1NjkxNC1jZmZiLTQ3OGUtYmUwOS01NDBiZmE3MTM2NDAvIiwiaWF0IjoxNTk1NjEyOTQ5LCJuYmYiOjE1OTU2MTI5NDksImV4cCI6MTU5NTYxNjg0OSwiYWlvIjoiRTJCZ1lQaWMydU1kK29RaC9NKzBMcTNINS82RkF3QT0iLCJhcHBpZCI6ImQ4Njk1OTM2LWFjOTUtNDhiYi1hN2UzLTI4MWZjZDAxYTJmOSIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzQ1MDU2OTE0LWNmZmItNDc4ZS1iZTA5LTU0MGJmYTcxMzY0MC8iLCJvaWQiOiI4NzNjMjg2Zi1jOWQzLTRkNzctYWRlMy1jMDk0ZDFhMjkyY2MiLCJzdWIiOiI4NzNjMjg2Zi1jOWQzLTRkNzctYWRlMy1jMDk0ZDFhMjkyY2MiLCJ0aWQiOiI0NTA1NjkxNC1jZmZiLTQ3OGUtYmUwOS01NDBiZmE3MTM2NDAiLCJ1dGkiOiJvY3ZwQkh1cGFFT2xYczVqNWpVeEFBIiwidmVyIjoiMS4wIn0.1WIoWCZP3UXwTzfCyNzea6g8JEwWXN4mq36fveSye9M7VL_m_INJLFLh5PNgjtJZeo5WwxBkHzKFqtPRiFIOZxh2xR9O8kuRGdnFyOFFmBKzHJOogGXrwgfxf38MPYsvxTiPkZhRFGc2pUwzixdz3Kc-Cdh2XEQvUfoIrTaK7dfR5XmhMTsu-o4B-ThS60nGvuoPviAfkoooteqdkZETdPacrkrEAfw44vGPxnasBKvrrsvb9EIL6cB954dgAuJMsZaAwci2zRTZ4B1PAtUnrhJvLKIEUy0pDHsoi4kVjLIPkDFSAFVKgI-Ff7g6_eFKPHI7cU_BZGOznAMqw5M04A"
}
```


The JWT is stored within the access token if you split and base64 decode this access token the following is the payload:
```json
{
  "aud": "2ff814a6-3304-4ab8-85cb-cd0e6f879c1d",
  "iss": "https://sts.windows.net/45056914-cffb-478e-be09-540bfa713640/",
  "iat": 1595612949,
  "nbf": 1595612949,
  "exp": 1595616849,
  "aio": "E2BgYPic2uMd+oQh/M+0Lq3H5/6FAwA=",
  "appid": "d8695936-ac95-48bb-a7e3-281fcd01a2f9",
  "appidacr": "1",
  "idp": "https://sts.windows.net/45056914-cffb-478e-be09-540bfa713640/",
  "oid": "873c286f-c9d3-4d77-ade3-c094d1a292cc",
  "sub": "873c286f-c9d3-4d77-ade3-c094d1a292cc",
  "tid": "45056914-cffb-478e-be09-540bfa713640",
  "uti": "ocvpBHupaEOlXs5j5jUxAA",
  "ver": "1.0"
}
```

The identity of the JWT seems to be the `appid` property. 
This `appid` property is the same value as the `client_id` query param specified in the first `curl` request.


## Links
- [1618608993-jwt-types.md](1618608993-jwt-types.md)
