# 1603204075 ci-cd-credential-requirements

human makes commit -> CI/CD tools scans code (require API keys) -> build the code -> test the code (unit, integration or e2e) (require test credentials)  -> push the package to a package repository (artifactory) (requireds a credential) -> trigger CD process (new or update) (require a credential) -> setup infrastructure (require credentials) -> copy package/binary to infrasture (require credentials) -> run the applications (require SSH credentials) 



## Links
