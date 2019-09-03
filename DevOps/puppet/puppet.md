# Puppet 

* Installing puppet on AWS EC2  servers

# puppet 5.4 commands
```puppet config print modulepath --section master --environment producation```
```puppet agent --no-daemonize --server puppetmaster --onetime --verbose```
```puppet agent --no-daemonize --onetime --verbose```
```
vi /etc/puppet/puppet.conf

[main]
logdir=/var/log/puppet
vardir=/var/lib/puppet
ssldir=/var/lib/puppet/ssl
rundir=/var/run/puppet
factpath=$vardir/lib/facter
templatedir=$confdir/templates
prerun_command=/etc/puppet/etckeeper-commit-pre
postrun_command=/etc/puppet/etckeeper-commit-post
certificate_revocation = false
server=puppet-razor.karanja.local
environment = prod
[master]
# These are needed when the puppetmaster is run by passenger
# and can safely be removed if webrick is used.
ssl_client_header = SSL_CLIENT_S_DN
ssl_client_verify_header = SSL_CLIENT_VERIFY
```

```
puppet master conf file
[main]
server = puppetmaster
ssldir = /var/lib/puppet/ssl

[master]
always_cache_features = true
vardir = /var/lib/puppet
cadir  = /var/lib/puppet/ssl/ca
dns_alt_names = puppetmaster
```