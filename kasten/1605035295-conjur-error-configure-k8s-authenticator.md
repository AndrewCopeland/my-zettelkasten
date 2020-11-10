# 1605035295 conjur-error-configure-k8s-authenticator

```
`/root` is not writable.
Bundler will use `/tmp/bundler/home/unknown' as your home directory temporarily.
rake aborted!
Sequel::DatabaseError: PG::ReadOnlySqlTransaction: ERROR:  cannot execute INSERT in a read-only transaction
```

## Solution
- You may be executing the command on a follower or standby instance.




## Links
