# 1602127552 azure-rotating-service-principals
It is recommened to use this method to update a service princiapl password:
https://docs.microsoft.com/en-us/graph/api/serviceprincipal-addpassword?view=graph-rest-1.0&tabs=http


One thing to not is this method is called addPassword. And the documentation seems to back this up. 

This means one service principal could have multiple passwords at the same time. This could be helpful for situtations the require dual accounts.



This is how to remove a password:
https://docs.microsoft.com/en-us/graph/api/serviceprincipal-removepassword?view=graph-rest-1.0&tabs=http




## Links
