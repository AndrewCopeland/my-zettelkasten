# 1601308739 conjur-ec2-spring-boot-lift-and-shift
Spring boot applications have a general configuration file that typically contains credentials to connect to databases or other services. This can open the door to malicious actors retrieving credentials to obtain sensative information.

To resolve this issue one should abstract these secrets outside of the Spring boot config file and instead use environment variables.

Below we will go over how to remove hard coded credentials from your Spring boot applications.


First step is to make sure Conjur is already installed.


Second is to load the following policy:
```yaml
- !host spring-boot-app1

- &secrets
  - !variable db/username
  - !variable db/password

- !permit
  role: !host spring-boot-app1
  resources: *secrets
  privileges: [ read, execute ]
```

After loading the policy above the `!host spring-boot-app1` will have the ability to retrieve the secret values of `db/username` and `db/password`.






## Links
