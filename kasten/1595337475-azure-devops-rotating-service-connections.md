# 1595337475 azure-devops-rotating-service-connections
How to update a Service Endpoint:
https://docs.microsoft.com/en-us/rest/api/azure/devops/serviceendpoint/endpoints/update%20service%20endpoint?view=azure-devops-rest-5.1


## Generating a PAT (Personal Access Token)
To interface with the Azure DevOps REST API a PAT will have to be generated.
How to generate this PAT can be found here: https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page


To update Service Connections the PAT generated must have the following scope:
```
Service Connections
read, query & manage
```


## Querying Service Connections
At first we must query the service connections. In this example we can gind a specific service connection using a name however under the hood a ID is used to identity specific service connections.

To query a service connection with a specific name perform the following HTTP request:
```
GET https://dev.azure.com/{{organizationname}}/{{projectname}}/_apis/serviceendpoint/endpoints?api-version=5.1-preview.2&endpointNames={{serviceConnectionName}}
Authorization: Basic :{{Password}}
Content-Type: application/json
```
In this case the {{Password}} is the generated PAT, the colon is intentional because username does not need to provided when authenticating with a PAT.

After sending in the request above. We should receieve a response that looks like the following:

```json
{
    "count": 1,
    "value": [
        {
            "data": {
                "Host": "some.hostname.local",
                "Port": "22"
            },
            "id": "62fadb0d-4d25-4595-97d4-6bca28b5b1c3",
            "name": "andrew-ssh-service-connection",
            "type": "ssh",
            "url": "ssh://some.hostname.local:22",
            "createdBy": {
                "displayName": "Andrew Copeland",
                "url": "https://spsprodcus3.vssps.visualstudio.com/Aa967de97-ce76-428c-b942-718064879813/_apis/Identities/570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
                "_links": {
                    "avatar": {
                        "href": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
                    }
                },
                "id": "570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
                "uniqueName": "andcope1995@gmail.com",
                "imageUrl": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh",
                "descriptor": "msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
            },
            "description": "Andrews SSH Service connection",
            "authorization": {
                "parameters": {
                    "username": "andrew"
                },
                "scheme": "UsernamePassword"
            },
            "isShared": false,
            "isReady": true,
            "owner": "Library",
            "serviceEndpointProjectReferences": [
                {
                    "projectReference": {
                        "id": "428a351e-1791-4644-8f59-34b5968e2f24",
                        "name": "testing-conjur"
                    },
                    "name": "andrew-ssh-service-connection",
                    "description": "Andrews SSH Service connection"
                }
            ]
        }
    ]
}
```

We should check that only 1 result was returned since there should be a 1-1 mapping between name and service connection.

We can now parse the ID and make a direct request to this service connection. 
```
GET https://dev.azure.com/{{organization}}/{{project}}/_apis/serviceendpoint/endpoints/62fadb0d-4d25-4595-97d4-6bca28b5b1c3?api-version=5.1-preview.2

Authorization: Basic :{{Password}}
Content-Type: application/json
```

And the following will be returned:
```json
{
    "data": {
        "Host": "some.hostname.local",
        "Port": "22",
        "PrivateKey": null
    },
    "id": "62fadb0d-4d25-4595-97d4-6bca28b5b1c3",
    "name": "andrew-ssh-service-connection",
    "type": "ssh",
    "url": "ssh://some.hostname.local:22",
    "createdBy": {
        "displayName": "Andrew Copeland",
        "url": "https://spsprodcus3.vssps.visualstudio.com/Aa967de97-ce76-428c-b942-718064879813/_apis/Identities/570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
        "_links": {
            "avatar": {
                "href": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
            }
        },
        "id": "570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
        "uniqueName": "andcope1995@gmail.com",
        "imageUrl": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh",
        "descriptor": "msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
    },
    "description": "Andrews SSH Service connection",
    "authorization": {
        "parameters": {
            "username": "andrew",
            "password": null
        },
        "scheme": "UsernamePassword"
    },
    "isShared": false,
    "isReady": true,
    "owner": "Library",
    "serviceEndpointProjectReferences": [
        {
            "projectReference": {
                "id": "428a351e-1791-4644-8f59-34b5968e2f24",
                "name": "testing-conjur"
            },
            "name": "andrew-ssh-service-connection",
            "description": "Andrews SSH Service connection"
        }
    ]
}
```

From this returned result, we just need to update the `authorization.parameters.password` attribute to the new value of the SSH Key.

The following request is how to update the password field:
```
PUT https://dev.azure.com/{{organization}}/{{project}}/_apis/serviceendpoint/endpoints/62fadb0d-4d25-4595-97d4-6bca28b5b1c3?api-version=5.1-preview.2

Authorization: Basic :{{Password}}
Content-Type: application/json

{
    "data": {
        "Host": "some.hostname.local",
        "Port": "22"
    },
    "id": "62fadb0d-4d25-4595-97d4-6bca28b5b1c3",
    "name": "andrew-ssh-service-connection",
    "type": "ssh",
    "url": "ssh://some.hostname.local:22",
    "createdBy": {
        "displayName": "Andrew Copeland",
        "url": "https://spsprodcus3.vssps.visualstudio.com/Aa967de97-ce76-428c-b942-718064879813/_apis/Identities/570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
        "_links": {
            "avatar": {
                "href": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
            }
        },
        "id": "570a6bc3-cb5f-6346-a7c1-d8c7bd12857a",
        "uniqueName": "andcope1995@gmail.com",
        "imageUrl": "https://dev.azure.com/andcope1995/_apis/GraphProfile/MemberAvatars/msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh",
        "descriptor": "msa.NTcwYTZiYzMtY2I1Zi03MzQ2LWE3YzEtZDhjN2JkMTI4NTdh"
    },
    "description": "Andrews SSH Service connection",
    "authorization": {
        "parameters": {
            "username": "andrew",
            "password": "{{NEW_PASSWORD_GOES_HERE}}"
        },
        "scheme": "UsernamePassword"
    },
    "isShared": false,
    "isReady": true,
    "owner": "Library",
    "serviceEndpointProjectReferences": [
        {
            "projectReference": {
                "id": "428a351e-1791-4644-8f59-34b5968e2f24",
                "name": "testing-conjur"
            },
            "name": "andrew-ssh-service-connection",
            "description": "Andrews SSH Service connection"
        }
    ]
}
```

A 200 should be returned and the updating of the password was successful.

#Links
