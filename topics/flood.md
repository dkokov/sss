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

See [Netfilter/iptables SIP tutorial](../tutorials/netfilter.md) for simple and effective protection.


### UDP flood with SIP messages


### Links

* [UDPFlooder](http://www.hackingvoip.com/tools/udpflood.tar.gz)
