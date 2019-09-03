Thus, to get a complete presentation of the netfilter rules, you need

iptables -vL -t filter
iptables -vL -t nat
iptables -vL -t mangle
iptables -vL -t raw
iptables -vL -t security

iptables -S
iptables -L -t nat

iptables --table nat --list

# iptables-save > iptables_bckp
# vim iptables_bckp
# iptables-restore < iptables_bckp

```bash
#!/bin/bash

 echo "Filter table:"
 iptables -t filter -vL

 echo "Nat table:"
 iptables -t nat -vL

 echo "Mangle table:"
 iptables -t mangle -vL

 echo "Raw table:"
 iptables -t raw -vL

 echo "Security table:"
 iptables -t security -vL

 echo 
 echo "All rules in all tables printed"
 ```

remove rules


# flush all chains
iptables -F
iptables -t nat -F
iptables -t mangle -F
# delete all chains
iptables -X


## Clear ip6tables rules:
```bash
ip6tables -P INPUT ACCEPT
ip6tables -P FORWARD ACCEPT
ip6tables -P OUTPUT ACCEPT
ip6tables -t nat -F
ip6tables -t mangle -F
ip6tables -F
ip6tables -X
```