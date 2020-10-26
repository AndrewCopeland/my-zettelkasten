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
```
# /opt/puppetlabs/puppet/lib/ruby/vendor_ruby/puppet/parser/functions/api_key_retreiver.rb
require 'conjur/api'

module Puppet::Parser::Functions
  newfunction(:api_key_retreiver, :type => :rvalue) do |args|
    # I will need to test the automatic retrieval of Conjur configuration below.
	# I was not able to get it to work in my lab and might just have the wrong file permisisons
    # Conjur::Config.load
    # Conjur::Config.apply
    # conjur = Conjur::Authn.connect nil, noask: true
    domain = args[0]
    pp_role = args[1]
    pp_app_tier = args[2]

    Conjur.configuration.account = 'conjur'
    Conjur.configuration.appliance_url = 'https://conjur-master'
    OpenSSL::SSL::SSLContext::DEFAULT_CERT_STORE.add_file "/etc/conjur-conjur.pem"
	
	# Of coarse we do not want a hardcoded API key in the function itself. 
	# This was a work around since I Was not able to get the conjur.conf to load automatically.
    conjur = Conjur::API.new_from_key "host/master1", "3d26y791vfc8xx29cm5x71gp799dfdygbk3wsdxec2m73rpw18bdksz"

    desiredHostname = "#{domain}-#{pp_role}-#{pp_app_tier}"
    resources = conjur.resources kind: 'variable', search: desiredHostname

    if resources.length() == 0
      raise "No resources were returned withj query '#{desiredHostname}'"
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

So now we have imported the function lets modify the `site.pp` or the `init.pp` to propogate the API key returned to the function down to the puppet agent node.
```
# /etc/puppetlabs/code/environments/production/manifests/site.pp or /etc/puppetlabs/code/environments/production/manifests/init.pp
$result = api_key_retreiver($domain, $pp_role, $pp_app_tier)
$login = $result[login]
$apiKey = $result[apiKey]

file { "/etc/conjur.identity":
        mode => "0644",
        content  => "machine https://conjur-master/authn\n  login $login \n  password $apiKey \n",
}
```

Since we updated the function we will need to reload/restart the puppet master:
```bash
/etc/init.d/puppetserver reload
```

Now run `puppet agent -t` on the puppet agent node and the `/etc/conjur.identity` should be created with the correct `login` and `password`.


## Links
