# OpenSIPS security tutorial

* [Introduction](#Introduction)

* [Links](#Links)

## Introduction

**"OpenSIPS is a multi-functional, multi-purpose signaling SIP server used by carriers, 
telecoms or ITSPs for solutions like Class4/5 Residential Platforms, 
Trunking / Wholesale, Enterprise / Virtual PBX Solutions, Session Border Controllers, 
Application Servers, Front-End Load Balancers, IMS Platforms, Call Centers, and many others."**

It's the first sentense by OpenSIPS project web page.

Realy this server can do a lot :-)
My expirence with OpenSIPS is starting before seven years.
I was in Chicago on ClueCon2012 and I included in OpenSIPS traing then.
I have seen VLAD's presentation by a conference.
It was my first OpenSIPS security release :-).

So...I'm using the OpenSIPS as Load Balancer with SBC functionality.
The Load Balancing will be into several FreeSWITCHes.
The FreeSWITCH keeps my media streams and release my voice services.

## OpenSIPS config (opensips.cfg)

```
# Thread protection - Malformed packets (according to SIP RFC3261)
if(!sipmsg_validate()) {
    $var(ret) = $retcode;
    xlog("L_WARN","$ci|log|Bad Request/Body from $si $rm ($fu),retcode=$var(ret)");

    if($var(ret) == -2) {
        if($var(d)) xlog("L_INFO", "$ci|debug|Header Parsing error - DROP!");
        drop;
    } else sl_send_reply("400", "Bad Request/Body");

    exit;
}
```

## Links

* [fail2ban](https://github.com/fail2ban/fail2ban)
* [OpenSIPS](https://opensips.org/)
* [VLAD PAIU,OpenSIPS,Securing SIP Networks](https://opensips.org/pub/events/2012-08-07_ClueCon_Chicago/VLAD_PAIU-OpenSIPS-Securing_SIP_Networks.pdf)