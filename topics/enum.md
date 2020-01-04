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
nmap -sU -p 5060 192.168.0.32/27
```

See results for this scanning:
``` bash
...

Nmap scan report for 192.168.0.44
Host is up (0.0015s latency).

PORT     STATE  SERVICE
5060/udp closed sip

....

Nmap scan report for 192.168.0.59
Host is up (0.0018s latency).

PORT     STATE         SERVICE
5060/udp open          sip

Nmap scan report for 192.168.0.60
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
nmap -sT -p 5060,5061 192.168.0.11
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

<!-- sip

svmap.py send to 192.168.11.54.5060 follow message:

OPTIONS sip:100@192.168.11.54 SIP/2.0
Via: SIP/2.0/UDP 127.0.0.1:5060;branch=z9hG4bK-1279845454;rport
Content-Length: 0
From: "sipvicious"<sip:100@1.1.1.1>;tag=3265326637663336313363340131333436363034303138
Accept: application/sdp
User-Agent: Zoiper
To: "sipvicious"<sip:100@1.1.1.1>
Contact: sip:100@127.0.0.1:5060
CSeq: 1 OPTIONS
Call-ID: 260690755765159327651007
Max-Forwards: 70


192.168.11.54.5060 send to svmap.py follow response:

SIP/2.0 200 OK
Via: SIP/2.0/UDP 127.0.0.1:5060;branch=z9hG4bK-1279845454;rport=5060;received=185.4.80.9
From: "sipvicious" <sip:100@1.1.1.1>;tag=3265326637663336313363340131333436363034303138
To: "sipvicious" <sip:100@1.1.1.1>;tag=jQ9Z467r94FSm
Call-ID: 260690755765159327651007
CSeq: 1 OPTIONS
Contact: <sip:192.168.11.54>
User-Agent: FreeSWITCH-mod_sofia/1.6.6~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Content-Length: 0

-->




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

<!--
14:05:20.299004 IP (tos 0x0, ttl 60, id 45255, offset 0, flags [DF], proto UDP (17), length 421)
    185.4.80.9.5060 > 46.47.127.54.5060: SIP, length: 393
	INVITE sip:100@46.47.127.54 SIP/2.0
	Via: SIP/2.0/UDP 127.0.0.1:5060;branch=z9hG4bK-2238248617;rport
	Content-Length: 0
	From: "sipvicious"<sip:100@1.1.1.1>;tag=3265326637663336313363340132373632313034383732
	Accept: application/sdp
	User-Agent: Zoiper
	To: "sipvicious"<sip:100@1.1.1.1>
	Contact: sip:100@127.0.0.1:5060
	CSeq: 1 INVITE
	Call-ID: 413441024767659234813129
	Max-Forwards: 70
	
	
14:05:20.299264 IP (tos 0x0, ttl 64, id 1581, offset 0, flags [none], proto UDP (17), length 381)
    46.47.127.54.5060 > 185.4.80.9.5060: SIP, length: 353
	SIP/2.0 100 Trying
	Via: SIP/2.0/UDP 127.0.0.1:5060;branch=z9hG4bK-2238248617;rport=5060;received=185.4.80.9
	From: "sipvicious" <sip:100@1.1.1.1>;tag=3265326637663336313363340132373632313034383732
	To: "sipvicious" <sip:100@1.1.1.1>
	Call-ID: 413441024767659234813129
	CSeq: 1 INVITE
	User-Agent: FreeSWITCH-mod_sofia/1.6.6~64bit
	Content-Length: 0
	
	
14:05:20.301667 IP (tos 0x0, ttl 64, id 1582, offset 0, flags [none], proto UDP (17), length 879)
    46.47.127.54.5060 > 185.4.80.9.5060: SIP, length: 851
	SIP/2.0 407 Proxy Authentication Required
	Via: SIP/2.0/UDP 127.0.0.1:5060;branch=z9hG4bK-2238248617;rport=5060;received=185.4.80.9
	From: "sipvicious" <sip:100@1.1.1.1>;tag=3265326637663336313363340132373632313034383732
	To: "sipvicious" <sip:100@1.1.1.1>;tag=UHy2KaKgFU89m
	Call-ID: 413441024767659234813129
	CSeq: 1 INVITE
	User-Agent: FreeSWITCH-mod_sofia/1.6.6~64bit
	Accept: application/sdp
	Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
	Supported: timer, path, replaces
	Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
	Proxy-Authenticate: Digest realm="1.1.1.1", nonce="e3ff4d9a-ca4c-11e9-8d2a-ebdf9d5de328", algorithm=MD5, qop="auth"
	Content-Length: 0

-->

<!-- #### **NOTIFY** Username Enumeration -->


## Links

* "HACKING VoIP",HIMANSHU DWIVEDI
* [Nmap ("Network Mapper")](https://nmap.org/)
* [SIPVicious Blog](http://blog.sipvicious.org/)