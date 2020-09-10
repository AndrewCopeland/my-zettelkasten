# 1599754328 secretless-k8s-manifest-example

The following is an example of the secrets k8s manifest.


```yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: secretless-account
  namespace: demoapps

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: secretless-config
  namespace: demoapps
data:
  secretless.yml: |
    version: "2"
    services:
      postgres-db:
        protocol: pg
        listenOn: tcp://0.0.0.0:5432 # can be a socket as well (same name for both)
        credentials:
          host: secretless-db.demoapps.svc.cluster.local
          port: 5432
          sslmode: disable
          password:
            from: conjur
            get: secrets/backend/postgres_pwd
          username:
            from: conjur
            get: secrets/backend/postgres_user
        config:  # this section usually blank
          optionalStuff: foo

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secretless
  namespace: demoapps
  labels:
    app: secretless
spec:
  replicas: 1
  selector:
    matchLabels:
      role: demo
      app: secretless
  template:
    metadata:
      labels:
        role: demo
        app: secretless
    spec:
      serviceAccountName: secretless-account
      shareProcessNamespace: true
      containers:
      - name: secretless-broker
        image: cyberark/secretless-broker:latest
        args: ["-f", "/etc/secretless/secretless.yml"]
        imagePullPolicy: Always
        ports:
        - containerPort: 5432
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
          - name: CONJUR_AUTHN_URL
            value: https://access.dap.svc.cluster.local/api/authn-k8s/k8s-follower
          - name: CONJUR_ACCOUNT
            value: cyberark
          - name: CONJUR_VERSION
            value: '5'
          - name: CONJUR_AUTHN_LOGIN
            value: host/conjur/authn-k8s/k8s-follower/apps/demoapps/deployment/secretless
          - name: CONJUR_SSL_CERTIFICATE
            valueFrom:
              configMapKeyRef:
                name: k8s-app-ssl
                key: ssl-certificate
        volumeMounts:
          - mountPath: /etc/secretless
            name: config
            readOnly: true
          - mountPath: /run/conjur
            name: conjur-access-token
      - name: secretless-app
        image: captainfluffytoes/secretlessapp:latest
        command: ["sleep","infinity"]
        imagePullPolicy: IfNotPresent
        env:
        volumeMounts:
          - mountPath: /run/conjur
            name: conjur-access-token
      volumes:
        - name: conjur-access-token
          emptyDir:
            medium: Memory
        - name: config
          configMap:
            name: secretless-config
            defaultMode: 420

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secretless-db
  namespace: demoapps
  labels:
    app: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      role: demo
      app: postgres
  template:
    metadata:
      labels:
        role: demo
        app: postgres
    spec:
      serviceAccountName: secretless-account
      shareProcessNamespace: true
      containers:
      - name: secretless-db
        image: postgres
        imagePullPolicy: IfNotPresent
        env:
          - name: POSTGRES_PASSWORD
            value: Cyberark1
          - name: POSTGRES_USER
            value: db_user
          - name: POSTGRES_DB
            value: petstore
        ports:
          - containerPort: 5432
            protocol: TCP

---
apiVersion: v1
kind: Service
metadata:
  name: secretless-db
  namespace: demoapps
spec:
  ports:
  - name: postgres
    port: 5432
    targetPort: 5432
  selector:
    app: postgres
  type: ClusterIP
```



## Links
