# Installing Puppet Master

## Puppet Master on Ubuntu 18.04
### Setup Prerequisites
> One of the key requirements of the Puppet master is network time synchronization.  
>We will ensure we have correct timezone set on the Puppet master server as well as working NTP service. We will later configure Agent nodes to sync their time with the Puppet Master,

#### Step 1: Set correct timezone
- setting time zone for puppet master server
    ```
    timedatectl set-timezone Asia/Jerusalem
    ```
- Checking time zone 
    ```
    timedatectl 
    ```

#### Step 2: Set server hostname
- Use the hostnamectl command to set server hostname
    ```
    hostnamectl set-hostname puppet-server
    ```

- Add correct hostnames and IP addresses we’ll use later to /etc/hosts file.
    ```
    # echo "192.168.1.2 puppet-server.<domain_name> puppet-server " >> /etc/hosts
    # echo "192.168.1.3 node-01.<domain_name> node-01" >> /etc/hosts
    ```

#### Step 3: Set ntp service
- installing **ntp** package for time sync
    ```
    apt-get -y install ntp
    ```
- If you would like to restrict which systems can use your ntp server, add a line like below to **/etc/ntp.conf**
    ```
    restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
    ```
  Replace **192.168.1.0** with your trusted network.
- The restart **ntp** service:
    ```
    systemctl restart ntp
    ```

### Install Puppet Master on Ubuntu 
> Now that all prerequisites are met, proceed to download PuppetLabs repository for Ubuntu 18.04 and Install Puppet master on the server.
- adding puppet repository to system 18.04 Bionic Beaver
    ```
    $ sudo apt-get install wget
    $ wget https://apt.puppetlabs.com/puppet-release-bionic.deb
    $ sudo dpkg -i puppet-release-bionic.deb 
    Selecting previously unselected package puppet-release.
    (Reading database ... 100156 files and directories currently installed.)
    Preparing to unpack puppet-release-bionic.deb ...
    Unpacking puppet-release (1.0.0-2bionic) ...
    Setting up puppet-release (1.0.0-2bionic) ...

    $ apt update
    ```
- adding puppet repository to system 16.04 Bionic Beaver
    ```
    wget https://apt.puppetlabs.com/puppetlabs-release-pc1-xenial.deb
    sudo dpkg -i puppetlabs-release-pc1-xenial.deb
    sudo apt-get update 
    ```
- installing puppet server pacakges
    ```
    apt-get install puppetmaster -y 
    ```
    Confirm the installed version of Puppet
    ```
    apt policy puppetmaster
    puppetmaster:
    Installed: 5.4.0-2ubuntu3
    Candidate: 5.4.0-2ubuntu3
    Version table:
    *** 5.4.0-2ubuntu3 500
        500 http://mirrors.digitalocean.com/ubuntu bionic/universe amd64 Packages
        500 http://mirrors.digitalocean.com/ubuntu bionic/universe i386 Packages
        100 /var/lib/dpkg/status
    ```
    On Ubuntu, the service should be started automatically:
    ```
    root@puppet-server:~# systemctl status puppet-master.service
    WARNING: terminal is not fully functional
    * puppet-master.service - Puppet master
    Loaded: loaded (/lib/systemd/system/puppet-master.service; enabled; vendor preset: enabled)
    Active: active (running) since Sat 2019-03-30 08:31:35 IDT; 7min ago
        Docs: man:puppet-master(8)
    Main PID: 18384 (puppet)
        Tasks: 3 (limit: 2320)
    CGroup: /system.slice/puppet-master.service
            `-18384 /usr/bin/ruby /usr/bin/puppet master

    Mar 30 08:31:34 puppet-server puppet-master[18337]: Signed certificate request for ca
    Mar 30 08:31:34 puppet-server puppet-master[18337]: puppet-server has a waiting certificate request
    Mar 30 08:31:34 puppet-server puppet-master[18337]: Signed certificate request for puppet-server
    Mar 30 08:31:34 puppet-server puppet-master[18337]: Removing file Puppet::SSL::CertificateRequest puppet-server at '/var/lib/puppet/ssl/ca/requests/puppet-server.pem'
    Mar 30 08:31:34 puppet-server puppet-master[18337]: Removing file Puppet::SSL::CertificateRequest puppet-server at '/var/lib/puppet/ssl/certificate_requests/puppet-server.pem'
    Mar 30 08:31:34 puppet-server puppet-master[18337]: The WEBrick Puppet master server is deprecated and will be removed in a future release. Please use Puppet Server instead. See h
    Mar 30 08:31:34 puppet-server puppet-master[18337]:    (location: /usr/lib/ruby/vendor_ruby/puppet/application/master.rb:207:in `main')
    Mar 30 08:31:35 puppet-server puppet-master[18384]: Reopening log files
    Mar 30 08:31:35 puppet-server puppet-master[18384]: Starting Puppet master version 5.4.0
    Mar 30 08:31:35 puppet-server systemd[1]: Started Puppet master.
    ```

### Configure Puppet Master on Ubuntu 18.04
> After the Puppet master server has been installed, it is time to start the configuration. It is recommended to change Puppet Java process memory allocation Infrastructure size. I’ll assign my Puppet server 1gb of ram

- edit puppet environment file **/etc/default/puppet-master**
    ```
    vim /etc/default/puppet-master
    JAVA_ARGS="-Xms1024m -Xmx1024m"
    ```
- restarting puppet master service
    ```
    $ sudo systemctl restart puppet-master.service
    ```
- Use the puppet config command to set values for the dns_alt_names setting
  ```
  /opt/puppetlabs/bin/puppet config set dns_alt_names 'puppet,puppet.example.com' --section main
  ```
- Start and enable the puppetserver service:
    ```
    systemctl start puppet-server
    systemctl enable puppet-server
    ```
    checking service exposed 
    ```
    netstat -anpl | grep 8140
    ```
#### Configure Firewall:
- if firewall is enabled on your system add the fallowing ports to allow in the firewall rule
    ```
    sudo ufw allow 8140/tcp
    ``
    puppet should be restart after making those changes to ufw
#### Create test manifest
> To make this guide complete, we’re going to create a simple Puppet manifest to Install Apache web server on Ubuntu 18.04 client server. Start by creating a folder path for the nginx class:

- create module directory
    ```
    $ mkdir -p /etc/puppet/modules/nginx/manifests
    ```
