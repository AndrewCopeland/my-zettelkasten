# 1611590291 conjur-authn-iam-python-botocore

```python3
import json
import logging
import http.client
import base64
import requests
from botocore.auth import SigV4Auth
from botocore.awsrequest import AWSRequest
from botocore.credentials import get_credentials
from botocore.session import Session
from botocore.httpsession import URLLib3Session
REGION = "us-east-1"
SERVICE = "sts"
HOST = f"{SERVICE}.amazonaws.com"
URL = f"https://{HOST}/?Action=GetCallerIdentity&Version=2011-06-15"
METHOD = "GET"
CONTENT_TYPE = "application/x-www-form-urlencoded; charset=utf-8"
CONJUR = "ad2425c323ab54312b2e52011a6784f2-221758664.eu-central-1.elb.amazonaws.com"
creds = get_credentials(Session())
sigv4 = SigV4Auth(creds, SERVICE, REGION)
req = AWSRequest(method=METHOD, url=URL, headers={"Host": HOST, "Content-Type": CONTENT_TYPE})
sigv4.add_auth(req)
req = req.prepare()
headers = dict(req.headers)
authn_url = f"https://{CONJUR}/authn-iam/prod/cyberark/host%2FsecretApp%2F573339233868%2FTrustedWithSecrets/authenticate"
resp = requests.post(authn_url, data=json.dumps(headers), headers={"Accept-Encoding": "base64"} ,verify=False)
print(f"Code: {resp}")
print(f"Content: {resp.content}")
token = f'Token token ="{resp.content.decode("utf-8").strip()}"'
auth = {"Authorization": token}
secret_url = f"https://{CONJUR}/secrets/cyberark/variable/secretApp/connectionstring"
resp = requests.get(secret_url, verify=False, headers=auth)
print(f"Code: {resp}")
print(f"Content: {resp.content}")
```


## Links
