# 1617713931 docker-logs-location

The default logging location can be found here:
```
/var/lib/docker/containers/[container-id]/[container-id]-json. log.
```

To get the log path execute the following:
```bash
docker inspect [container-id] | findstr "LogPath"
```



## Links
