# Flooding Attacks

  Every SIP proxy/server/router have to guard from UDP flooding attacks,
because a lot of stream from udp messages into the server will be block
normal working of anybody.The server cannot response on others requests.
**Will be the classic DoS attack!!!**

* [UDP flood with noSIP messages](udp-flood-with-nosip-messages)
* [UDP flood with SIP messages](udp-flood-with-sip-messages)
* [TCP flood with noSIP messages](tcp-flood-with-nosip-messages)
* [TCP flood with SIP messages](tcp-flood-with-sip-messages)

* [Links](#Links)

### UDP flood with noSIP messages

``` bash
./udpflood src_ipaddr dst_ipaddr src_port dst_port packets_number
```

See [Netfilter/iptables SIP tutorial](../tutorials/netfilter.md) for simple protection.

### UDP flood with SIP messages

  You can be used '**./sss.php**' for this case.
See ['**man tools**'](../man/tools.md) for more details
  

  If you copy some SIP message,for example somebody **OPTIONS** message,
You can be sent to your SIP proxy in loop mode,for to test this case.

``` bash
./sss.php -f tmpl/notify.tmpl flood
```

  This script can generate flood with load _**~ 7800-8200 req/s**_ !!!
It's the really DoS attack !!! You can do any preventions !!!

See [OpenSIPS security tutorial](../tutorials/opensips.md) for simple protection.

### TCP flood with noSIP messages

``` bash
```

See [Netfilter/iptables SIP tutorial](../tutorials/netfilter.md) for simple protection.

### TCP flood with SIP messages

  You can be used '**./sss.php**' for this case.
See ['**man tools**'](../man/tools.md) for more details
  

  If you copy some SIP message,for example somebody **OPTIONS** message,
You can be sent to your SIP proxy in loop mode,for to test this case.

``` bash
./sss.php -f tmpl/notify.tmpl flood
```

  This script can generate flood with load _**~ 7800-8200 req/s**_ !!!
It's the really DoS attack !!! You can do any preventions !!!

See [OpenSIPS security tutorial](../tutorials/opensips.md) for simple protection.

### Links

* [UDPFlooder](http://www.hackingvoip.com/tools/udpflood.tar.gz)
