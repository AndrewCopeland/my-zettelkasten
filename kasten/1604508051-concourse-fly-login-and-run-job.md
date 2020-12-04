# 1604508051 concourse-fly-login-and-run-job

This example shows logging into concourse using fly. Creating a team, setting up a job and running and watching the output of that job:
```bash
platform=linux
curl "http://10.0.8.5:8080/api/v1/cli?arch=amd64&platform=$type" -o fly
chmod +x ./fly
./fly -t ci login --concourse-url http://10.0.8.5:8080 -u admin -p admin
./fly -t ci set-team --team-name "operations:ops-sbx" --local-user admin --non-interactive
./fly -t ci login --concourse-url http://10.0.8.5:8080 -n "operations:ops-sbx" -u admin -p admin
./fly -t ci set-pipeline -p test-pipeline -c "$(pwd)/pipelines/pipeline.yml" -n
./fly -t ci unpause-pipeline -p test-pipeline
./fly -t ci trigger-job -j test-pipeline/job-conjur-api-key
./fly -t ci watch -j test-pipeline/job-conjur-api-key
```


## Links
