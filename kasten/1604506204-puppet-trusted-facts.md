# 1604506204 puppet-trusted-facts
Trusted facts are extracted from the node's certificate. Which guarantees accuracy so the node cannot change the actual fact.

https://puppet.com/docs/puppet/5.5/lang_facts_and_builtin_vars.html#trusted-facts

The can be refrenced using the `$trusted` hash. 

The trusted hash typically looks like:
```
{
  'authenticated' => 'remote',
  'certname'      => 'web01.example.com',
  'domain'        => 'example.com',
  'extensions'    => {
                      'pp_uuid'                 => 'ED803750-E3C7-44F5-BB08-41A04433FE2E',
                      'pp_image_name'           => 'storefront_production'
                      '1.3.6.1.4.1.34380.1.2.1' => 'ssl-termination'
                   },
  'hostname'      => 'web01'
}
```

To get the `pp_image_name` you can use the following code:
```
$trusted['extensions']['pp_image_name']
```


## Links
- [1603725828-puppet-dual-api-key-implementation.md](1603725828-puppet-dual-api-key-implementation.md)
- [1603725828-puppet-dual-api-key-implementation.md](1603725828-puppet-dual-api-key-implementation.md)
- [1603725828-puppet-dual-api-key-implementation.md](1603725828-puppet-dual-api-key-implementation.md)
