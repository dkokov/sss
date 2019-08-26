# Flooding Attacks

  Every SIP proxy/server/router have to guard from UDP flooding attacks,
because a lot of stream from udp messages into the server will be block
normal working of anybody.The server cannot response on others requests.
**Will be the classic DoS attack!!!**

* [UDP flood with noSIP messages](udp-flood-with-nosip-messages)
* [UDP flood with SIP messages](udp-flood-with-sip-messages)
* [Links](#Links)

### UDP flood with noSIP messages

``` bash
./udpflood src_ipaddr dst_ipaddr src_port dst_port packets_number
```

A variant for simple protection with NETFILTER/IPTABLES:

``` iptables

-N sip
-N sip_reject


-A INPUT -p udp -m recent --name SIP --rcheck --seconds 60 --dport 5060 -j sip_reject
-A INPUT -p udp --dport 5060 -j sip

-A sip -p udp -m string --hex-string "|0d 0a 0d 0a|" --from 28 --to 32 --algo bm --dport 5060 -j ACCEPT
-A sip -p udp -m string --string !"sip:" --algo bm --dport 5060 -j sip_reject
-A sip -p udp -m string --string "sip:" --algo bm --dport 5060 -j ACCEPT
-A sip -p udp --dport 5060 -j sip_reject

-A sip_reject -p udp -m udp -m recent --set --name SIP --dport 5060 -j REJECT --reject-with icmp-host-prohibited

```

### UDP flood with SIP messages


### Links

* [UDPFlooder](http://www.hackingvoip.com/tools/udpflood.tar.gz)
