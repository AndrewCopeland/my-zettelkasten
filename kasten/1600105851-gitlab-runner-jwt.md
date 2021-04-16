# 1600105851 gitlab-runner-jwt
Everytime a gitlab job is kicked off it is injected with an environment variable called: `CI_JOB_JWT`.

This environment variable is what the runner uses to identify the specific job that is running. An example of this JWT is below:
```
eyJhbGciOiJSUzI1NiIsImtpZCI6Imtld2lRcTlqaUM4NEN2U3NKWU9CLU42QThXRkxTVjIwTWIteTdJbFdEU1EiLCJ0eXAiOiJKV1QifQ.eyJuYW1lc3BhY2VfaWQiOiI5MzYwNTE4IiwibmFtZXNwYWNlX3BhdGgiOiJhbmRjb3BlMTk5NSIsInByb2plY3RfaWQiOiIyMTEyOTU1NyIsInByb2plY3RfcGF0aCI6ImFuZGNvcGUxOTk1L3Rlc3QtcnVubmVyIiwidXNlcl9pZCI6IjcwODA5ODYiLCJ1c2VyX2xvZ2luIjoiYW5kY29wZTE5OTUiLCJ1c2VyX2VtYWlsIjoiYW5kY29wZTE5OTVAZ21haWwuY29tIiwicGlwZWxpbmVfaWQiOiIxODk3OTc4NzEiLCJqb2JfaWQiOiI3MzcxNTE2NjUiLCJyZWYiOiJtYXN0ZXIiLCJyZWZfdHlwZSI6ImJyYW5jaCIsInJlZl9wcm90ZWN0ZWQiOiJ0cnVlIiwianRpIjoiZGI1ZjUzMmUtMjQzNi00ZmFlLTlhNGUtYjU3YzU0NmNmMDQ4IiwiaXNzIjoiZ2l0bGFiLmNvbSIsImlhdCI6MTYwMDEwNTc3NCwibmJmIjoxNjAwMTA1NzY5LCJleHAiOjE2MDAxMDkzNzQsInN1YiI6ImpvYl83MzcxNTE2NjUifQ.FCVgaWSy5iX6PaMLe6r-WjnjS5RggBDpCJEW_K7KGwRSvsqvNdTI_Gsbntz_NhL3yWXfCwi5erStqMMCCsOsJOBG5ze8D7I5qKeQMU_rM9BfRk_em49JozSV2uXihDxqEBHXaHOdgtEjjQ7xPAGkzdv-mqSoSJ3ZKsvUi3i28lrriRksJSwIsQZ-sWn46F29GTc-f-DDFqf70PIZRIw9deDC0Kln2zlnzX19Mr1xJZ-Bc0fO6fX9wvUvVBPdwv9Ml7hnKgkXICFmBVaDe9rgK4_0n6e8BnZAdWH1_SsNTI0402WGutLZlDCVCw4J4pVh2jRgKRG681j7wrgJLmJM2k6QKUOUM_z7a4eZlD2SkH99CHmmouQq4MVgko8hC9asuFCiQDL_UM6KiBFKsJBeuWwtZ05zJn-9plbJ356vDYeu2NGjYChRDo5r-lf2FPKyD5Zb4sOGqujDmj9mjYXz1Z3yI5u_rneQWS3PEYiro2HKkpeVgK5GOlF0nRFyYnvchW7Pt0-UUYygFxyN2EfGRR6MTDCjOXYRPINUCUNuoZHFa2NECTW1haTGQZlQh1ifnPFoiaIHifBMhGRPAufEUIij5BJkDZGtn2G02fIMkVLzYJSFmXyalY9WGxWnBF3FHzlNAf_6_wAsMM8dKGZpzxBL7BDnpvAsXIXa7k1D9IE
```


The payload of this JWT after being base64 decoded is the following:
```
{
  "namespace_id": "9360518",
  "namespace_path": "andcope1995",
  "project_id": "21129557",
  "project_path": "andcope1995/test-runner",
  "user_id": "7080986",
  "user_login": "andcope1995",
  "user_email": "andcope1995@gmail.com",
  "pipeline_id": "189797871",
  "job_id": "737151665",
  "ref": "master",
  "ref_type": "branch",
  "ref_protected": "true",
  "jti": "db5f532e-2436-4fae-9a4e-b57c546cf048",
  "iss": "gitlab.com",
  "iat": 1600105774,
  "nbf": 1600105769,
  "exp": 1600109374,
  "sub": "job_737151665"
}
```



## Links
- [1600106175-conjur-authn-gitlab.md](1600106175-conjur-authn-gitlab.md)
- [1618608993-jwt-types.md](1618608993-jwt-types.md)
