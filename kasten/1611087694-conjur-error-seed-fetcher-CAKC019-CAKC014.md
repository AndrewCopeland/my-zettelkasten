# 1611087694 conjur-error-seed-fetcher-CAKC019-CAKC014

The following error message is being returned when attempting to use the seedfile fetcher.
```
DAP Seed Fetcher v0.1.7
Trying to fetch seedfile from https://<redacted>/configuration/nibr/seed/follower...
Hostname is --- conjur-follower ---
Using master ssl cert from /tmp/master.crt
Calculated vars:
- CONJUR_APPLIANCE_URL: https://<redacted>
- CONJUR_AUTHN_URL: https://<redacted>/authn-k8s/dappoc
- CONJUR_AUTHN_LOGIN: host/dappoc/dap-authn-service
- WGET_CERT_ARGS: --ca-certificate
Running authenticator...
INFO: 2021/01/19 19:14:49.707778 main.go:16: CAKC048 Kubernetes Authenticator Client v0.19.0-4d6d0c8 starting up...
DEBUG: 2021/01/19 19:14:49.707821 logger.go:60: CAKC052 Debug mode is enabled
ERROR: 2021/01/19 19:14:50.208938 client.go:16: CAKC014 Failed to to append Conjur CA cert
ERROR: 2021/01/19 19:14:50.209005 main.go:75: CAKC019 Failed to instantiate authenticator object
```

The error appears to stem from [cyberark/conjur-authn-k8s-client](https://github.com/cyberark/conjur-authn-k8s-client)
Error `CAKC014` is from [this line](https://github.com/cyberark/conjur-authn-k8s-client/blob/ac965f440218c36d273fba541f3462f69916ebc0/pkg/authenticator/client.go#L16)
Error `CAKC019` is from [this line](https://github.com/cyberark/conjur-authn-k8s-client/blob/eddf71f39e560f336afe18003737a205a371c5b0/cmd/authenticator/main.go#L28)


The following error `CAKC014` is returned because `AppendCertsFromPEM` is returning with failure.
```go
ok := caCertPool.AppendCertsFromPEM(CACert)
if !ok {
	return nil, log.RecordedError(log.CAKC014)
}
```

Documentation on the AppendCertsFromPEM function can be found [here](https://golang.org/pkg/crypto/x509/#CertPool.AppendCertsFromPEM)


### Solution?
- If this error occurs I would validate the `CONJUR_SSL_CERTIFICATE` environment variables in the dap follower manifest.




I set my `CONJUR_SSL_CERTIFICATE` using a config map. The target config map I used looks like:
```
$ kubectl -n <conjur-follower-namespace> get configmap conjur-config -o yaml
apiVersion: v1
data:
  CONJUR_MASTER_CERTIFICATE: |-
    -----BEGIN CERTIFICATE-----
    MIIDcTCCAlmgAwIBAgIVAJPh45JJEPcvTxv/H8mBRNPNCKwuMA0GCSqGSIb3DQEB
    CwUAME4xETAPBgNVBAoMCGN5YmVyYXJrMRIwEAYDVQQLDAlDb25qdXIgQ0ExJTAj
    BgNVBAMMHHNvdXJjZS5kYXAuc3ZjLmNsdXN0ZXIubG9jYWwwHhcNMjEwMTEzMTMx
    MTU1WhcNMzEwMTExMTMxMTU1WjAnMSUwIwYDVQQDDBxzb3VyY2UuZGFwLnN2Yy5j
    bHVzdGVyLmxvY2FsMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAy0SA
    A+Fe9zyAsiEKid9Mi9amXR7FaNK45EYBmAbTk7VhPyDC2vmbzkUZJ0Mkv4v6dp56
    ZDQ+3wJieGrN5W4++uMcKn2v6trxMkDXzII3ZfDM4zchLoqQR+FuuG+g+hgu1bxy
    QIgXQw/JxDSYPtqFN034gc3+AzIFLWCuHOUecC9Bu/K2S3IbZOkRRX61Iqf9G34h
    zoyiAgg/puRYHdYZnlgvvulB19p6pcHiZoW6zkRtdNkyO/80VMKTnx1owUxC8RO2
    X+GG8H1n/HMvFAm9vgakCEEiHsaxbZwQNrj2gERjc9zaRSWLVxN2xOCuTqHWnyu4
    O8Zm/0MmnzxolB1p3QIDAQABo20wazAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0OBBYE
    FNo5o+5ea0sNMlW/75VgGJCv2AcJMDoGA1UdEQQzMDGCHHNvdXJjZS5kYXAuc3Zj
    LmNsdXN0ZXIubG9jYWyCCWxvY2FsaG9zdIIGY29uanVyMA0GCSqGSIb3DQEBCwUA
    A4IBAQDhxt2splPCdO1sBKq1c58Cbm6EciVwmrB4f77uyzngY1WheX+DD2HRHjX5
    t235dO0V4Z895s7OEqnIWCocAFWc4lUQh6PQHjIF1tX2D9fbAlmHX/z2k6lmLAO5
    3PHG2b7SE4p4Egh3Bc7eTz4WxSLd0HhUQzPjVXKVC50nKzfxsldOUWlzWhCT3yJN
    cqjv281hr+txkhuXic5uyF4vN03mWAhg3LpY4fzhcNuGtMrV0NIgdoz0NWNBStgq
    Z7V8FE/LFI3yrBfkFLFN7rr3EOBXaoNwzfvS+okN3i4iJ1WAG9XPRP3lTOrlbcOY
    gqWKJGcMYaYMPlveiT1vj3T9U3zy
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    MIIDxTCCAq2gAwIBAgIUNOpjg7OrqHA35tuOU6SxKR9WgrgwDQYJKoZIhvcNAQEL
    BQAwTjERMA8GA1UECgwIY3liZXJhcmsxEjAQBgNVBAsMCUNvbmp1ciBDQTElMCMG
    A1UEAwwcc291cmNlLmRhcC5zdmMuY2x1c3Rlci5sb2NhbDAeFw0yMTAxMTMxMzEx
    NTBaFw0zMTAxMTExMzExNTBaME4xETAPBgNVBAoMCGN5YmVyYXJrMRIwEAYDVQQL
    DAlDb25qdXIgQ0ExJTAjBgNVBAMMHHNvdXJjZS5kYXAuc3ZjLmNsdXN0ZXIubG9j
    YWwwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQD1acD8amlLVZmauOZu
    BUkhtg5Ac2mjtmjPfpvQeXJ0tXkgjInPOW0YqIeXMxd+KVl1OiGp8Rj6LMOqStZh
    vo6fyB85B64tMPLN6QB4oXqD9JKPVUvyOBTVtWwCELf1yx4Fv7uSjfZ/+x25ktEf
    PAPWVDFZlBh3RC7E9GgwZ3JZj5FClo1fnS1izBvYTeqYwLSuNbG1nhI18ZU16Vie
    46qBQVnrWt0CH2JCKaGERhqYSodpmhMxXCzsK23kCDTczF45B479BkIueKPupigv
    5c9K236bmtqaqA1PxmZLHjASN8Q+ZiQmzy2Db9ziTglxGVKm+k9ANL9+STbNkUjz
    wibNAgMBAAGjgZowgZcwOgYDVR0RBDMwMYIcc291cmNlLmRhcC5zdmMuY2x1c3Rl
    ci5sb2NhbIIJbG9jYWxob3N0ggZjb25qdXIwHQYDVR0OBBYEFHJp9k/II4PjXtK+
    ruJrKytJ0ezxMB8GA1UdIwQYMBaAFHJp9k/II4PjXtK+ruJrKytJ0ezxMAwGA1Ud
    EwQFMAMBAf8wCwYDVR0PBAQDAgHmMA0GCSqGSIb3DQEBCwUAA4IBAQALQap6xeEV
    Kr4VWq7s4sUs9QnaJADs30DMNZOZPaBr8GmJGxnQeTCBH37y7Ajz4MYm/cNY675d
    nHDZdwU/vXzs9hQTj8yKLnPsDIinu+ZGrmTR+8ZS3QVXENytQcYLPLO03GL1xX0L
    AwrDOXWI+5JFwmkkcLAT9HXzdGNNHM7X2y5IN3XJhS9ynNJMwd+Vi/ddmhJ7SfUq
    H1kTou8/Qh+6hW8NDXiHv3/JQLaVe02S/nBjdTYMTQeuGoQYLLUBpuKIkPKqSf89
    Jz523mTp5ABJhC7SxhTLS0BE7D2nPjFv7OIy4ACPgVAKxOB27UZwi9RRCXN7k3w8
    Z4r46Q8QbHbe
    -----END CERTIFICATE-----
kind: ConfigMap
metadata:
  creationTimestamp: "2021-01-13T13:14:02Z"
  managedFields:
  - apiVersion: v1
    fieldsType: FieldsV1
    fieldsV1:
      f:data:
        .: {}
        f:ssl-certificate: {}
    manager: kubectl
    operation: Update
    time: "2021-01-13T13:14:02Z"
  name: k8s-app-ssl
  namespace: dap
  resourceVersion: "83183"
  selfLink: /api/v1/namespaces/dap/configmaps/k8s-app-ssl
  uid: b196fa09-b459-4885-b3d1-c7e10b8fd60a
```


## Links
