# 1599582165 postgres-create-table

```
CREATE TABLE certificate
(
    serialNumber VARCHAR(100) NOT NULL,
    expirationDate INTEGER NOT NULL,
    certificate VARCHAR(1000) NOT NULL,
    revoked BOOLEAN DEFAULT FALSE,
    revocationDate INTEGER DEFAULT 0,
    revocationReasonCode INTEGER DEFAULT 0, 
    CONSTRAINT certificate_pkey PRIMARY KEY (serialNumber)
);
```

The following SQL statement can be applied to a postgres datase. In the schema above will have the following rules associated with it:
- `serialNumber`, `expirationDate`, `certificate` columns are required
- `revoked` defaults to `False`
- `revocationDate` and `revocationReasonCode` defaults to `0`
- `serialNumber` will represent the Primary Key (Should not have the ability to create multiple certificates with the same serialNumber)


## Links
- [1599582410-postgres-create-user.md](1599582410-postgres-create-user.md)
- [1599582635-postgres-grant-user-connect.md](1599582635-postgres-grant-user-connect.md)
