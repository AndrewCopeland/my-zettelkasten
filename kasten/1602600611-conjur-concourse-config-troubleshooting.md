# 1602600611 conjur-concourse-config-troubleshooting
It is possible to validate all of the needed environment variables required for Concourse to use conjur.

To do this SSH into the concourse web VM and cat out the bpm.yml file.


Get list of VMs:
```
bosh vms
```

SSH into a specific VM:
```
bosh ssh -d <deployment name> <id of the VM>
```

cat out the `bpm.yml` file and list all of the conjur variables
```
cat /var/vcap/jobs/web/config/bpm.yml | grep CONJUR
```

## Links
