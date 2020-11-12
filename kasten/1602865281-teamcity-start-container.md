# 1602865281 teamcity-start-container

```bash
docker run -it --name teamcity-server-instance  \
    -v $HOME/tmp/teamcity/data:/data/teamcity_server/datadir \
    -v $HOME/tmp/teamcity/logs:/opt/teamcity/logs  \
    -p 8111:8111 \
    jetbrains/teamcity-server
```

## Links
- [1605203636-teamcity-docker-setup-agent.md](1605203636-teamcity-docker-setup-agent.md)
