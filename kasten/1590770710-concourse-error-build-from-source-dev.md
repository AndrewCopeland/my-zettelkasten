# 1590770710 concourse-error-build-from-source-dev
#concourse #error #build #source #dev

I recieveing this error:
```
web_1     | {"timestamp":"2020-05-29T16:05:04.368561700Z","level":"info","source":"atc","message":"atc.cmd.finish","data":{"duration":172100,"session":"1"}}
web_1     | panic: reflect: call of reflect.Value.Call on zero Value
web_1     |
web_1     | goroutine 1 [running]:
web_1     | reflect.flag.mustBe(0x0, 0x13)
web_1     | 	/usr/local/go/src/reflect/value.go:208 +0xde
web_1     | reflect.Value.Call(0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0)
web_1     | 	/usr/local/go/src/reflect/value.go:319 +0x52
web_1     | github.com/concourse/concourse/atc/db/migration/migrations.(*migrations).Run(0xc0002e2000, 0xc000741121, 0xd, 0x0, 0x0)
web_1     | 	/src/atc/db/migration/migrations/migrations.go:21 +0xfb
web_1     | github.com/concourse/concourse/atc/db/migration.(*migrator).runMigration(0xc0002260a0, 0xc000741121, 0xd, 0x5e2882af, 0xc00076ea1a, 0x2, 0x0, 0x0, 0x0, 0x0, ...)
web_1     | 	/src/atc/db/migration/migration.go:317 +0xe6
web_1     | github.com/concourse/concourse/atc/db/migration.(*migrator).Migrate(0xc0002260a0, 0x5ec559d7, 0x0, 0x0)
web_1     | 	/src/atc/db/migration/migration.go:266 +0x8e8
web_1     | github.com/concourse/concourse/atc/db/migration.(*migrator).Up(0xc0002260a0, 0x0, 0x0)
web_1     | 	/src/atc/db/migration/migration.go:370 +0x14e
web_1     | github.com/concourse/concourse/atc/db/migration.(*OpenHelper).Open(0xc0004fd4c0, 0x0, 0x0, 0x0)
web_1     | 	/src/atc/db/migration/migration.go:63 +0x1da
web_1     | github.com/concourse/concourse/atc/db.Open(0x3d56d00, 0xc00020b440, 0x37081a9, 0x1d, 0xc000130850, 0x68, 0x0, 0x0, 0x36d1133, 0x3, ...)
web_1     | 	/src/atc/db/open.go:65 +0x125
web_1     | github.com/concourse/concourse/atc/atccmd.(*RunCommand).constructDBConn(0xc000468800, 0x37081a9, 0x1d, 0x3d56d00, 0xc00020afc0, 0x20, 0x36d1133, 0x3, 0x3cd7360, 0xc0004fd440, ...)
web_1     | 	/src/atc/atccmd/command.go:1312 +0x205
web_1     | github.com/concourse/concourse/atc/atccmd.(*RunCommand).Runner(0xc000468800, 0xc0000ceb90, 0x0, 0x1, 0x0, 0x0, 0x0, 0x0)
web_1     | 	/src/atc/atccmd/command.go:430 +0xb25
web_1     | main.(*WebCommand).Runner(0xc000508d88, 0xc0000ceb90, 0x0, 0x1, 0x0, 0x0, 0x0, 0x0)
web_1     | 	/src/cmd/concourse/web.go:51 +0x13b
web_1     | main.(*WebCommand).Execute(0xc000508d88, 0xc0000ceb90, 0x0, 0x1, 0x0, 0x0)
web_1     | 	/src/cmd/concourse/web.go:36 +0x85
web_1     | github.com/vito/twentythousandtonnesofcrudeoil.installEnv.func2(0x7f343be5b2a8, 0xc000508d88, 0xc0000ceb90, 0x0, 0x1, 0x0, 0x0)
web_1     | 	/go/pkg/mod/github.com/vito/twentythousandtonnesofcrudeoil@v0.0.0-20180305154709-3b21ad808fcb/environment.go:40 +0x13b
web_1     | github.com/jessevdk/go-flags.(*Parser).ParseArgs(0xc0004f0cc0, 0xc0000ea030, 0x1, 0x1, 0x0, 0x0, 0x0, 0x0, 0x0)
web_1     | 	/go/pkg/mod/github.com/jessevdk/go-flags@v1.4.0/parser.go:314 +0xcdf
web_1     | github.com/jessevdk/go-flags.(*Parser).Parse(0xc0004f0cc0, 0x0, 0x0, 0x0, 0x0, 0x0)
web_1     | 	/go/pkg/mod/github.com/jessevdk/go-flags@v1.4.0/parser.go:186 +0xf5
web_1     | main.main()
web_1     | 	/src/cmd/concourse/main.go:30 +0x27e
concourse_web_1 exited with code 2
```

And when building the concourse cnotainer I would get this error:
```
Step 11/19 : RUN concourse generate-key -t rsa -b 1024 -f /concourse-keys/session_signing_key
 ---> Running in 33042f327b42
error: invalid argument for flag `--tsa-worker-private-key' (expected *flag.PrivateKey): failed to read private key file (/concourse-keys/worker_key): open /concourse-keys/worker_key: no such file or directory
```



## Links
- [1590770855-concourse-solution-upgrade-dev-image.md](1590770855-concourse-solution-upgrade-dev-image.md)
- [1587417961-conjur-concourse-integration-with-bosh.md](1587417961-conjur-concourse-integration-with-bosh.md)
