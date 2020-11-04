# 1603725828 puppet-dual-api-key-implementation
First we must download the dependancies required for the puppet master to interface with the Conjur API:
```bash
puppetserver gem install conjur-api
puppetserver gem install conjur-cli
```

If you run into issues running the command above you may need it install the following dependancies:
```bash
apt-get install build-essential g++
```

After installing these dependancies we can now import the function that will run on the compile master:
```ruby
# /opt/puppetlabs/puppet/lib/ruby/vendor_ruby/puppet/parser/functions/api_key_retriever.rb
require 'conjur/api'
require 'conjur/cli'

module Puppet::Parser::Functions
  newfunction(:api_key_retriever, :type => :rvalue) do |args|
    domain = args[0]
    pp_role = args[1]
    pp_app_tier = args[2]

    # -- file
    Conjur::Config.load
    Conjur::Config.apply
    conjur = Conjur::Authn.connect nil, noask: true


    desiredHostname = "#{domain}-#{pp_app_tier}-#{pp_role}"
    resources = conjur.resources kind: 'variable', search: desiredHostname

    if resources.length() == 0
      raise "No resources were returned with query '#{desiredHostname}'"
    elsif resources.length() > 1
      raise "More than on resource was returned with query '#{desiredHostname}'"
    else
       variable = resources[0]
       return {
         'login' => "host/#{desiredHostname}",
	 'apiKey' => "#{variable.value}"
       }
    end
  end
end
```

Lets create the files in which the puppet master will get its identity from.
Create a file `/etc/conjur.conf` with the following content:
```
---
account: conjur
plugins: []
appliance_url: https://conjur-follower-url
cert_file: "/etc/conjur-conjur.pem"
netrc_path: "/etc/conjur.identity"
```

Create the file that represents the conjur certificate file `/etc/conjur-conjur.pem`:
```bash
openssl s_client -showcerts -connect <conjur instance host name>:443 < /dev/null 2> /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /etc/conjur-conjur.pem
```

Create the identity file that contains the api key `/etc/conjur.identity`:
```
machine https://conjur-follower-url/authn
  login host/<host name>
  password <host api key>
```


So now we have imported the function and created the config and identity files lets modify the `site.pp` or the `init.pp` to propogate the API key returned to the function down to the puppet agent node.
```
$result = api_key_retriever($domain, $pp_role, $pp_app_tier)
notice($result)
$login = $result[login]
$apiKey = $result[apiKey]

$sslcert = @("EOT")
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
|-EOT

$dbpass = Deferred(conjur::secret, ['db/password', {
  appliance_url => "https://conjur-master",
  account => "conjur",
  authn_login => "$login",
  authn_api_key => Sensitive("$apiKey"),
  ssl_certificate => $sslcert
}])


file { "/etc/db.password":
        mode => "0644",
        content  => $dbpass,
}
```

Since we updated the function/gems required by the function we need to reload/restart the puppet master:
```bash
/etc/init.d/puppetserver reload
```

Now run `puppet agent -t` on the puppet agent node and the `/etc/conjur.identity` should be created with the correct `login` and `password`.


## Links
- [1603470643-puppet-master-defining-catalog-for-agents.md](1603470643-puppet-master-defining-catalog-for-agents.md)
- [1604348394-conjur-puppet-policy.md](1604348394-conjur-puppet-policy.md)
- [1604506204-puppet-trusted-facts.md](1604506204-puppet-trusted-facts.md)
