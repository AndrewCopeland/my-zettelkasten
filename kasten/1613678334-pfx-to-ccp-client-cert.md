# 1613678334 pfx-to-ccp-client-cert

Extract the client certificate, key and certificate serial number from a PFX file.

```bash
# File path to the pfx file
PFX_FILE="CCP-ClientCert.pfx"

# do not modify below here
PEM_FILE="client-cert.pem"
CERT_FILE="client.crt"

openssl pkcs12 -in "${PFX_FILE}" -out "${PEM_FILE}" -nodes

privateKeyStart=$(grep -in "BEGIN PRIVATE KEY" client-cert.pem | awk -F ':' '{print $1}')
privateKeyEnd=$(grep -in "END PRIVATE KEY" client-cert.pem | awk -F ':' '{print $1}')

echo "\nPRIVATE KEY:"
sed -n "${privateKeyStart},${privateKeyEnd}p" < client-cert.pem

certKeyStart=$(grep -in "BEGIN CERTIFICATE" client-cert.pem | awk -F ':' '{print $1}')
certKeyEnd=$(grep -in "END CERTIFICATE" client-cert.pem | awk -F ':' '{print $1}')
echo "\nPUBLIC CERT:"
sed -n "${certKeyStart},${certKeyEnd}p" < client-cert.pem > $CERT_FILE
cat "${CERT_FILE}"

echo "\nCertificate Serial Number:"
openssl x509 -in "${CERT_FILE}" -serial -noout | awk -F "=" '{print $2}'

rm "${PEM_FILE}" "${CERT_FILE}"
```


## Links
