# 1603400158 pki-service-cli-usage

```bash
#
./cyberark-aam-pkiaas logon -u https://conjur-master -a conjur -c ~/tmp/conjur.pem -l admin
./cyberark-aam-pkiaas init
./cyberark-aam-pkiaas generate-csr -c pki-service -k rsa -b 2048 -m 1440 --self-signed
./cyberark-aam-pkiaas server -c pki1.service.company.local
```



## Links
