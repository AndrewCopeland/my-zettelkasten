# 1599754147 secretless-psql-command

Run the following command within a container that has the secretless broker as a side car.

This command will connect to the database without needing the correct address, username or password.

```bash
psql "host=localhost port=5432 user=db_user dbname=petstore sslmode=disable"
```

## Links
- [1599754328-secretless-k8s-manifest-example.md](1599754328-secretless-k8s-manifest-example.md)
