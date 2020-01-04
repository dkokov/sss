# Enumeration

* [Enumerating SIP Devices on a Network](#enumerating-sip-devices-on-a-network)
* [Username Enumeration](#username-enumeration)
* [Links](#Links)

## Enumerating SIP Devices on a Network

  Maybe the attacker will make network map with SIP devices/servers in the your network.
That means to enumerating your network - have or not ane SIP devices/servers.
For this task can be uses a **NMAP**.

See example command for UDP scanning to network:

``` bash
nmap -sU -p 5060 192.168.11.32/27
```

See results for this scanning:
``` bash
...

Nmap scan report for 192.168.11.44
Host is up (0.0015s latency).

PORT     STATE  SERVICE
5060/udp closed sip

....

Nmap scan report for 192.168.11.59
Host is up (0.0018s latency).

PORT     STATE         SERVICE
5060/udp open          sip

Nmap scan report for 192.168.11.60
Host is up (0.0018s latency).

PORT     STATE         SERVICE
5060/udp open|filtered sip

```

The **NMAP** send two UDP packets without data(only UDP header) to destination ip address and UDP port.
If the receiver don't response,maybe **DROP** these packets,STATE will be '**open|filtered**'.
Can response with some **REJECT** options.For example '**icmp-host-prohibited**',STATE will be '**filtered**'.
**REJECT** with '**icmp-port-unreachable**', STATE will be '**closed**'.

When the result is with STATE '**open**' or '**filtered**' can suppose that have a really SIP device.


See example command for TCP scanning to network:

``` bash
nmap -sT -p 5060,5061 192.168.11.54
```

See results for this scanning:
``` bash

Starting Nmap 7.70 ( https://nmap.org ) at 2019-08-29 15:09 EEST
Host is up (0.0011s latency).

PORT     STATE    SERVICE
5060/tcp closed   sip
5061/tcp filtered sip-tls

Nmap done: 1 IP address (1 host up) scanned in 0.27 seconds

```

## Username Enumeration

  Some tools don't need from the previous map,because able to insert directly a network as param.
Then the tool send SIP messages to several supposing SIP devices and make map after response,if have.

SIPVicious's tool **svmap.py**' is such.

<!--* Enumerating SIP Usernames with Error Messages.-->
* [**OPTIONS** Username Enumeration](#options-username-enumeration)
* [**REGISTER** Username Enumeration](#register-username-enumeration)
* [**INVITE**  Username Enumeration](#invite-username-enumeration)
<!--* [**NOTIFY**  Username Enumeration](notify-username-enumeration)-->


#### **OPTIONS** Username Enumeration

```
./svmap.py dest_ipaddr or network
```

See result from the mapping of '192.168.11.32/27' network:
``` bash

| SIP Device         | User Agent                       | Fingerprint |
----------------------------------------------------------------------
| 192.168.11.54:5060 | FreeSWITCH-mod_sofia/1.6.6~64bit | disabled    |

```

It's mean that '**svmap.py**' finds a SIP device - FreeSWITCH,version 1.6.6 !
When a tool don't find nothing,then see follow message in the console: **WARNING:root:found nothing**


#### **REGISTER** Username Enumeration
   

#### **INVITE** Username Enumeration

  You able to make a map as send directly INVITE message.
Then you will waiting to receive '**SIP/2.0 407 Proxy Authentication Required**' response.

See the command in this case:
```
./svmap.py 192.168.11.32/27 -m INVITE**
```

If have SIP devices in the network,will be inserted in the list.As this SIP device:

``` bash

| SIP Device        | User Agent                       | Fingerprint |
----------------------------------------------------------------------
| 192.168.11.54:5060 | FreeSWITCH-mod_sofia/1.6.6~64bit | disabled    |

```

<!-- #### **NOTIFY** Username Enumeration -->


## Links

* "HACKING VoIP",HIMANSHU DWIVEDI
* [Nmap ("Network Mapper")](https://nmap.org/)
* [SIPVicious Blog](http://blog.sipvicious.org/)