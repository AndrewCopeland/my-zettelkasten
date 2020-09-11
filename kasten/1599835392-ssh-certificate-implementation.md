# 1599835392 ssh-certificate-implementation

```bash
# does the public cert require a specific file permission
curl http://pkiaas:8080/ca/certificate > /etc/ssh/ca.pub

# adding the correct sshd_config and restarting
echo "TrustedUserCAKeys /etc/ssh/ca.pub" >> sshd_config
service ssh restart
```



## Links
