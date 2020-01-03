# Netfilter/iptables SIP tutorial

* [Introduction](#Introduction)
* [SIP iptables rules](#sip-iptables-rules)
* [Test cases](#test-cases)
* [Links](#Links)

### Introduction

  Should be say that don't have VoIP security as different topic from main topic security.
First have to guard same host.The host have to be very "hard".
Follow this is the VoIP security.It's follow step in your security politics.

  Don't talk about the guard of other your network services here.
In the Internet has a lot of tutorials for iptables rules per different services or 
example firewalls.


### SIP iptables rules

Let's go to start **iptables** configuration.

``` bash
vim /etc/sysconfig/iptables
```

Part of the example SIP **iptables** config:

``` iptables
# create new chains: sip and sip_reject
-N sip
-N sip_reject

-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# SIP rules,forwarding to chains 'sip' and 'sip_reject'
-A INPUT -p udp -m recent --name SIP --rcheck --seconds 60 --dport 5060 -j sip_reject
-A INPUT -p udp --dport 5060 -j sip

# Rule for 'Zoiper' keep-alive noSIP packets...maybe and for others softphones
-A sip -p udp -m string --hex-string "|0d 0a 0d 0a|" --from 28 --to 32 --algo bm --dport 5060 -j ACCEPT

# Rule for noSIP messages check
-A sip -p udp -m string --string !"sip:" --algo bm --dport 5060 -j sip_reject
-A sip -p udp -m string --string "sip:" --algo bm --dport 5060 -j ACCEPT
-A sip -p udp --dport 5060 -j sip_reject

-A sip_reject -p udp -m udp -m recent --set --name SIP --dport 5060 -j REJECT

```

See current SIP connections in the **NETFILTER** conntrack

``` bash
cat /proc/net/nf_conntrack | grep dport=5060

ipv4     2 udp      17 254 src=192.168.11.246 dst=192.168.127.46 sport=5222 dport=5060 [UNREPLIED] src=192.168.127.46 dst=192.168.11.246 sport=5060 dport=5222 mark=0 secmark=0 use=2
ipv4     2 udp      17 179 src=192.168.127.46 dst=192.168.127.42 sport=5060 dport=5060 src=192.168.127.42 dst=192.168.127.46 sport=5060 dport=5060 [ASSURED] mark=0 secmark=0 use=2
ipv4     2 udp      17 176 src=192.168.123.166 dst=192.168.127.46 sport=1139 dport=5060 src=192.168.127.46 dst=192.168.123.166 sport=5060 dport=1139 [ASSURED] mark=0 secmark=0 use=2
```

### Test cases

  Do you remember '**Flooding Attacks**' and '**Enumeration**' topics ?
If you don't read,first go to see ['**Flooding Attacks**'](../topics/flood.md) and ['**Enumeration**'](../topics/enum.md) topics.

Repeat again examples from these topics. Compare whether results are defferent.


### Links

[Netfilter/iptables project web page](https://netfilter.org/)
