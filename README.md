startrek
========

Spencer Krum and William Van Hevelingen

This module demonstrates the use of data-in-modules from Puppet 3.3.0+

As described in [ARM-9](http://links.puppetlabs.com/arm9-data_in_modules)

First verify that you have Puppet 3.3

Then turn on module bindings by adding the following to puppet.conf

```ini
parser = future
```

Put the following in your /etc/puppet/hiera.yaml

```yaml
---
version: 2
hierarchy:
  [ ['osfamily', '${osfamily}', '${osfamily}' ],
    ['environment', '${environment}', '${environment}' ],
    ['common', 'true', 'common' ]
  ]
backends:
  - yaml
  - json
```

Put the following in startrek/hiera.yaml, yes thats the hiera.yaml directly in your module root

```yaml
---
version: 2
```


Next create a test.pp file, if you used the puppet module tool you should already have this in tests/init.pp 

```puppet
include startrek
```

Create a datadir in your module:

```bash
cd startrek
mkdir data
cd data
```

```bash
cat data/common.yaml
```

```yaml
---
ops: spock
tactical: chekov
helm: sulu
communications: uhura
captain: kirk
```

```bash
cat data/osfamily/Debian.yaml
```

```yaml
---
tactical: tuvok
ops: kim
helm: paris
engineering: torres
captain: janeway
```

```bash
cat data/osfamily/RedHat.yaml
```

```yaml
---
tactical: worf
ops: data
helm: wesley
engineering: geordi
captain: picard
```


```bash
$ FACTER_OSFAMILY=RedHat puppet apply  tests/init.pp
Notice: Compiled catalog for pro-puppet.lan in environment production in 0.23 seconds
Notice: picard
Notice: /Stage[main]/Startrek/Notify[picard]/message: defined 'message' as 'picard'
Notice: Finished catalog run in 0.11 seconds

$ FACTER_OSFAMILY=Debian puppet apply  tests/init.pp
Notice: Compiled catalog for pro-puppet.lan in environment production in 0.23 seconds
Notice: janeway
Notice: /Stage[main]/Startrek/Notify[janeway]/message: defined 'message' as 'janeway'
Notice: Finished catalog run in 0.10 seconds

$ FACTER_OSFAMILY=ArchLinux puppet apply  tests/init.pp
Notice: Compiled catalog for pro-puppet.lan in environment production in 0.22 seconds
Notice: kirk
Notice: /Stage[main]/Startrek/Notify[kirk]/message: defined 'message' as 'kirk'
Notice: Finished catalog run in 0.11 seconds
```

License
-------

Apache 2

Contact
-------

Spencer Krum and William Van Hevelingen

Support
-------

Proof of concept, no support, not for production
