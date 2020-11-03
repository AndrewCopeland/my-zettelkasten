# 1604441748 error-conjur-concourse-fetching-secret
```
<13>1 2020-11-03T22:06:05.333+00:00 ce952da7e806 nginx - - [meta sequenceId="1063"] 172.20.22.110 "GET /secrets/DEV/concourse/operations/ops-sbx%2Fconjurtest%2Fintegration HTTP/1.1" 404 269 "-" "Go-http-client/1.1" 0.006 0.006
<13>1 2020-11-03T22:06:05.333+00:00 ce952da7e806 nginx - - [meta sequenceId="1064"] 172.20.22.110 "GET /secrets/DEV/concourse/operations/ops-sbx%2Fintegration HTTP/1.1" 404 246 "-" "Go-http-client/1.1" 0.006 0.006
<13>1 2020-11-03T22:06:05.333+00:00 ce952da7e806 nginx - - [meta sequenceId="1065"] 172.20.22.110 "GET /secrets/DEV/concourse/operations/ops-sbx%2Fintegration HTTP/1.1" 404 246 "-" "Go-http-client/1.1" 0.006 0.006
```

It looks like the concourse integration is not using the correct path to retrieve the secret.

This is the endpoint:
```
/secrets/DEV/concourse/operations/ops-sbx%2Fintegration
```

It is supposed to be
```
/secrets/DEV/variable/concourse/operations/ops-sbx%2Fintegration
```

Why is concourse behaving this way, I do not know since it did not work like this in my lab



## Links
