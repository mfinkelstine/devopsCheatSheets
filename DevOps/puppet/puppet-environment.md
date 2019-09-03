# Steps to enable environment settings on puppet
###Steps to enable environment settings on puppet

####On puppet master

1. Open puppet.conf and add below lines , modify the paths as per your environment
    ```
    environmentpath = /etc/puppet/code/environments
    default_manifest = /etc/puppet/code/manifests
    basemodulepath = /etc/puppet/code/modules
    ```
2. Create **production** folder inside environment folder
3. under **production** folder create a file **environment.conf** and set below values to it , change the path as per your environment
    ```
    modulepath = /etc/puppet/code/environments/production/modules
    manifest = /etc/puppet/code/environments/production/manifests
    ```
4. Now under **production** folder create above two folders that you specified inside environment.conf
5. Inside manifests folder create **site.pp** file and save it for now.
6. Under **module** folder create your **modules** , and include it as a class to **site.pp**
7. Now open **/etc/pupet.hiera.yaml** and add below lines under hierarchy
    ```
    :hierarchy:
    – “%{environment}”
    – “common”
    ```
    Here environment is the facter that we will set on puppet agent box.
also make sure you have :datadir set as
    ```
    :yaml:
    :datadir: /etc/puppet/hieradata
    ```

8. Now under hieradata folder create yaml files for respective environments and declare data inside it.
# On Puppet Agent
There are three ways we can make use of environments on puppet agent nodes.
## First way:
Once you have decided that which particular node will be production , staging or development.
run below command on respective agent
* For production
```puppet config set environment production –section agent```
* For Staging
```puppet config set environment staging –section agent```
* For Development
```puppet config set environment development –section agent```
After this run “puppet agent –test”
This will fetch the corresponding module meant for respective environment.

## Second Way
Once you have decided that which particular node will be production , staging or development.
Add below entries to .bash_profile file of the agent node
* For production
```export FACTER_environment=production```
* For Staging
```export FACTER_environment=staging```
* For Development
```export FACTER_environment=development```

After this run “puppet agent –test”
This will fetch the corresponding module meant for respective environment.

## Third Way
Once you have decided that which particular node will be production , staging or development.
use below command to get the changes from puppet master
* For production
```puppet agent –verbose –debug –onetime –no-daemonize –environment production```
* For Staging
```puppet agent –verbose –debug –onetime –no-daemonize –environment staging```
* For Development
```puppet agent –verbose –debug –onetime –no-daemonize –environment development```