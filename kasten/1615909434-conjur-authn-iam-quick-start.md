# 1615909434 conjur-authn-iam-quick-start

I run the following command to get the instance certificate:
```
openssl s_client -showcerts -connect localhost:8443 < /dev/null 2> /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'
-----BEGIN CERTIFICATE-----
MIIDcjCCAlqgAwIBAgIJAOWmUJOgRJh4MA0GCSqGSIb3DQEBCwUAMGUxCzAJBgNV
BAYTAlVTMRIwEAYDVQQIDAlXaXNjb25zaW4xEDAOBgNVBAcMB01hZGlzb24xETAP
BgNVBAoMCEN5YmVyQXJrMQ0wCwYDVQQLDARPbnl4MQ4wDAYDVQQDDAVwcm94eTAe
Fw0yMTAzMTYxNTQ4NTBaFw0yMjAzMTYxNTQ4NTBaMGUxCzAJBgNVBAYTAlVTMRIw
EAYDVQQIDAlXaXNjb25zaW4xEDAOBgNVBAcMB01hZGlzb24xETAPBgNVBAoMCEN5
YmVyQXJrMQ0wCwYDVQQLDARPbnl4MQ4wDAYDVQQDDAVwcm94eTCCASIwDQYJKoZI
hvcNAQEBBQADggEPADCCAQoCggEBAKqpQzjQk9DVnNh6L2Oc8THD5w9KlcVt8aJu
gDmVE5OeeDgcy98iQMbeGxhAcJmvnc6JIgk3cf48o3YatSvIBA6ej6Miwg0m3SjU
MgVdCI3GrHMGKmi1oGT//05IG0RSlEsoj5svABhTdfqSulbq79dnivRe1B8LzzN3
QUrr4vCIvh8tSHc3nbzufZ6y8Qlq/NYL36laX+XfBZ0eGXJ47BrxU91ilRj6bzoS
GKRLIXkIpmWdhfmSg7/DH1ZakKYSd5DHnu9h1InnIirgrH6NlxfwBhw5224APBDd
qveQoiDQ9+cW6MPrtEG8e3POlXDSC/hfPbryzn+uul6+SSU/hIcCAwEAAaMlMCMw
IQYDVR0RBBowGIIJbG9jYWxob3N0ggVwcm94eYcEfwAAATANBgkqhkiG9w0BAQsF
AAOCAQEAnsoeL8J16njlZHPoSfpwLYXGue4ZDgoPnCYCPqYq6eHsTBRc5SZ9tD0s
xr//8DzdLAGDgk1yCU0QNuR36StpUsOmY/0JUQq+zBXnBGOMRJUISC4NdC/4e+tt
KPZMv9vKt6WFzEjsuJ+Qws4gtI1HYwUPi0TXQ9DwY1EmArJ6jDosURTYqItSG4+x
6Rf6Toq1OOYYT2foaemgFBbmu7hvKkkoKJokhydgItOW5qclUwpHQCSd65jDU1Ru
UVwr/CB5gNfo7NsSBg+hdbYiwDtDcbulvOolu4AzGkp6RzT7eim4eu7XDlDg+vlH
4ZlWeeq3sjzfAd1TNzVrddroESCd3g==
-----END CERTIFICATE-----
```

Only one certificate is returned. In the enterprise version 2 certificates are returned within the chain.

I placed the certificate above into a file called `conjur.pem` and I was able to make a curl command to the conjur instance with the following command:
`curl https://localhost:8443 --cacert ./conjur.pem`

The return was the HTML front-page of conjur.


From here I will now load a policy that represents an aws host and give it access to some secrets:
```
- !policy
  id: conjur/authn-iam/global
  body:
  - !webservice
  - !group
  - !permit
    role: !group
    resource: !webservice
    privileges: [ read, authenticate ]

- &iam-hosts
  - !host 622705945757/ubuntu-client-conjur-identity

- &aws-secrets
  - !variable db/username
  - !variable db/password

# give all iam roles ability to authenticate
- !grant
  role: !group conjur/authn-iam/global
  members: *iam-hosts

# give all iam hosts access to the secrets
- !permit
  roles: *iam-hosts
  resources: *aws-secrets
  privileges: [ read, execute ]
```

After running the following command I can see what the common name and subject alternative names are:
```
$ curl https://localhost:8443 -vk
* Rebuilt URL to: https://localhost:8443/
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
* successfully set certificate verify locations:
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* TLSv1.2 (OUT), TLS header, Certificate Status (22):
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: C=US; ST=Wisconsin; L=Madison; O=CyberArk; OU=Onyx; CN=proxy
*  start date: Mar 16 15:48:50 2021 GMT
*  expire date: Mar 16 15:48:50 2022 GMT
*  issuer: C=US; ST=Wisconsin; L=Madison; O=CyberArk; OU=Onyx; CN=proxy
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
```

The accepted hostname are `proxy` or `localhost`. It would be nice to have this configurable when setting up the instance. 


I then downloaded the `conjur-authn-iam-client` to test this newly created identity: https://github.com/AndrewCopeland/conjur-authn-iam-client/releases/tag/v0.1.1

And then I ran the following command:
```
address="https://ec2-54-157-147-99.compute-1.amazonaws.com:8443"
./linux_authenticator -aws-name ec2 \
    -account myConjurAccount \
    -url "$address" \
    -authn-url "$address/authn-iam/global" \
    -login host/622705945757/ubuntu-client-conjur-identity \
    -ignore-ssl-verify
```

And then following was returned:
```
INFO:  2021/03/16 16:44:28.169631 config.go:60: AWS-Type: ec2; Account: myConjurAccount; Appliance URL: https://ec2-54-157-147-99.compute-1.amazonaws.com:8443; Login: host/622705945757/ubuntu-client-conjur-identity; Authn URL: https://ec2-54-157-147-99.compute-1.amazonaws.com:8443/authn-iam/global; Ignore SSL Verify: true; Access Token Path: ; Service ID: %!s(bool=false); Silence: %!v(MISSING)
INFO:  2021/03/16 16:44:28.169667 access_token.go:19: CAIC001I Retrieving AWS IAM credential for resource type 'ec2'
INFO:  2021/03/16 16:44:28.177490 access_token.go:24: CAIC002I Successfully retrieved AWS IAM credential
INFO:  2021/03/16 16:44:28.177619 access_token.go:31: CAIC003I Successfully created the Conjur authentication request
INFO:  2021/03/16 16:44:28.177650 access_token.go:34: CAIC004I Attempting to authenticate to conjur: authnUrl=https://ec2-54-157-147-99.compute-1.amazonaws.com:8443/authn-iam/global, login=host/622705945757/ubuntu-client-conjur-identity, account=myConjurAccount
INFO:  2021/03/16 16:44:28.177673 authenticate.go:28: CAIC005I Attempting to authenticate to conjur 'https://ec2-54-157-147-99.compute-1.amazonaws.com:8443/authn-iam/global/myConjurAccount/host%2F622705945757%2Fubuntu-client-conjur-identity/authenticate'
INFO:  2021/03/16 16:44:28.227030 access_token.go:39: CAIC006I Successfully authenticated to conjur with login 'host/622705945757/ubuntu-client-conjur-identity'
{"protected":"eyJhbGciOiJjb25qdXIub3JnL3Nsb3NpbG8vdjIiLCJraWQiOiJhYzYyNTI4YTUzM2Q3MTE1NzViZmExZTkwMzQyNjVhOWYwYmRjMmYwNzkwNjdkMTZhNjU5MDg4OTFlMTY2NjEyIn0=","payload":"eyJzdWIiOiJob3N0LzYyMjcwNTk0NTc1Ny91YnVudHUtY2xpZW50LWNvbmp1ci1pZGVudGl0eSIsImlhdCI6MTYxNTkxMzA2OH0=","signature":"qkQ1hfaYT_MmYiYyiOtJ5gpJs9_ILQ7t_QR5_UuAxkUNL633YIwAW1BQ8qtXvQvaXv2HxgQrC5yzCfxqcFzVowOJ1BVfuBs1EdRMhoctKin3_dfs9igI2U9eGUOvRsB0JH5BVy_HuZsgOrdMNeGQPdo_GnWxNjmg3d23ZS47lN4hMSmByBDAsmKxPY8EJEDBMwHQjfG0v7MeOtwsbJxgd246JOGpMlv3aj2WacuRyrEudpPTmUZDx4-xpCX5r9XX6JsqJjf8NWEK9Wuz729CoZ7Itttq5Q0zDmZVer8wGrRQXe32zCvPCnEfgce--NjlKDR-rTpHvghNITXllVKrs6YK88LHSzMqzcgh_b7OHuiwYDQcVTjnZ_E2cgR8dYO3"}
```

Then I took a look at the conjur logs:
```
[origin=172.18.0.3] [request_id=78898d76-9c1d-438f-a4c6-d864734ee43d] [tid=44] Started POST "/authn-iam/global/myConjurAccount/host%2F622705945757%2Fubuntu-client-conjur-identity/authenticate" for 172.18.0.3 at 2021-03-16 16:44:28 +0000
[origin=172.18.0.3] [request_id=78898d76-9c1d-438f-a4c6-d864734ee43d] [tid=44] Processing by AuthenticateController#authenticate as HTML
[origin=172.18.0.3] [request_id=78898d76-9c1d-438f-a4c6-d864734ee43d] [tid=44]   Parameters: {:controller=>"authenticate", :action=>"authenticate", :authenticator=>"authn-iam", :service_id=>"global", :account=>"myConjurAccount", :id=>"host/622705945757/ubuntu-client-conjur-identity"}
[origin=172.18.0.3] [request_id=78898d76-9c1d-438f-a4c6-d864734ee43d] [tid=44] myConjurAccount:host:622705945757/ubuntu-client-conjur-identity successfully authenticated with authenticator authn-iam service myConjurAccount:webservice:conjur/authn-iam/global
[origin=172.18.0.3] [request_id=78898d76-9c1d-438f-a4c6-d864734ee43d] [tid=44] Completed 200 OK in 31ms (Views: 0.2ms)
```



## Links
