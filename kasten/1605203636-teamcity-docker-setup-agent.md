# 1605203636 teamcity-docker-setup-agent

```bash
docker run -d -e SERVER_URL="http://teamcity-server-instance:8111" \
    --name teamcity-agent-instance \
    --network teamcity \
    -v $HOME/tmp/teamcity/agent/data/conf:/data/teamcity_agent/conf \
    jetbrains/teamcity-agent
```


## Links
- [1602865281-teamcity-start-container.md](1602865281-teamcity-start-container.md)
