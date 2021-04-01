# 1617294523 authn-k8s-obtain-k8s-lb-certificate
```bash
echo q | openssl s_client -connect <api.fqdn:8443> -servername <api.fqdn> -showcerts 2>/dev/null > api-chain.pem
```




## Links
