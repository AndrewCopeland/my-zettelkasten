# 1611335495 powershell-disable-ssl-verification

```powershell
Function Disable-SslVerification
{
try
{

add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    
    public class IDontCarePolicy : ICertificatePolicy {
        public IDontCarePolicy() {}
        public bool CheckValidationResult(
            ServicePoint sPoint, X509Certificate cert,
            WebRequest wRequest, int certProb) {
            return true;
        }
    }
"@
    [System.Net.ServicePointManager]::CertificatePolicy = new-object IDontCarePolicy 
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

}
catch
{
# Ignore if error is thrown, an error is thrown if we Disaable-SSL twice in the same pwoershell session
}
}
```


## Links
