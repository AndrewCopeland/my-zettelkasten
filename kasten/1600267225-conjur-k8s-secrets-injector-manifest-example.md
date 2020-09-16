# 1600267225 conjur-k8s-secrets-injector-manifest-example

The following manifest can be loaded to use the Kubernetes Secret Injector.

The benefit of this method is developers can use secrets just like normal k8s secrets, however instead of the developer actually loading the secrets a init container will inject the secrets at pod start up.


```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
  namespace: demoapps
type: Opaque
data:
  DBName:   bXlhcHBEQg==
stringData:
  conjur-map: |-   
    username: secrets/backend/postgres_pwd 
    password: secrets/backend/postgres_user

---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: k8ssecretdap-account
  namespace: demoapps

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: secrets-access
rules:
  - apiGroups: [""]
    resources: ["secrets"]
    verbs: [ "get", "patch" ]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: demoapps
  name: secrets-access-binding
subjects:
  - kind: ServiceAccount
    namespace: demoapps
    name: k8ssecretdap-account
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: secrets-access

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: k8ssecretdap
  namespace: demoapps
  labels:
    app: k8ssecretdap
spec:
  replicas: 1
  selector:
    matchLabels:
      role: demo
      app: k8ssecretdap
  template:
    metadata:
      labels:
        role: demo
        app: k8ssecretdap
    spec:
      serviceAccountName: k8ssecretdap-account
      shareProcessNamespace: true
      initContainers:
      - name: cyberark-secrets-provider
        image: captainfluffytoes/k8s-secret-synch:0.2.0
        imagePullPolicy: IfNotPresent
        env:
          - name: MY_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: MY_POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: MY_POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: CONJUR_APPLIANCE_URL
            value: https://access.dap.svc.cluster.local/api
          - name: CONTAINER_MODE
            value: init
          - name: CONJUR_AUTHN_URL
            value: https://access.dap.svc.cluster.local/api/authn-k8s/k8s-follower
          - name: CONJUR_ACCOUNT
            value: cyberark
          - name: CONJUR_VERSION
            value: '5'
          - name: CONJUR_AUTHN_LOGIN
            value: host/conjur/authn-k8s/k8s-follower/apps/demoapps/deployment/k8ssecretdap
          - name: SECRETS_DESTINATION
            value: k8s_secrets
          - name: K8S_SECRETS
            value: db-credentials
          - name: CONJUR_SSL_CERTIFICATE
            valueFrom:
              configMapKeyRef:
                name: k8s-app-ssl
                key: ssl-certificate
      containers:
      - name: k8s-app
        image: centos
        command: ["sleep","infinity"]
        imagePullPolicy: IfNotPresent
        env:
          - name: DB_USERNAME
            valueFrom:
              secretKeyRef:
                name: db-credentials
                key: username
          - name: DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: db-credentials
                key: password
```


## Links
- [1586274452-conjur-authn-k8s.md](1586274452-conjur-authn-k8s.md)
