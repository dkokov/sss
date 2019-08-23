# OpenSIPS security tutorial

* [Introduction](#Introduction)
* [Features](#Features)
* [Links](#Links)

## Introduction

_**"OpenSIPS is a multi-functional, multi-purpose signaling SIP server used by carriers, 
telecoms or ITSPs for solutions like Class4/5 Residential Platforms, 
Trunking / Wholesale, Enterprise / Virtual PBX Solutions, Session Border Controllers, 
Application Servers, Front-End Load Balancers, IMS Platforms, Call Centers, and many others."**_

It's the first sentense by OpenSIPS project web page.

Really this server can do a lot :-)
My expirence with OpenSIPS is starting before seven years.
I was in Chicago on ClueCon2012.I have included in OpenSIPS traing then.
I have seen VLAD's presentation by a conference.
It was my first OpenSIPS security release :-).

Let's go to see follow topology: The OpenSIPS as Load Balancer with SBC functionality.
The Load Balancing will be into several FreeSWITCHes.
The FreeSWITCH keeps media streams and release voice services.

Test cases are release with **OpenSIPS** _version 2.4.5_ and **FreeSWITCH** _version 1.6.20_

## Features

### Thread protection - Malformed packets (according to SIP RFC3261)

For this task,you can be used a follow OpenSIPS's function:

* [sipmsg_validate()](https://opensips.org/html/docs/modules/2.4.x/sipmsgops.html#func_sipmsg_validate)

``` php
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

When a '$retcode = -2',opensips cannot return response message.
Therefore in this case has 'drop;'.

You can test this case with follow command:

``` bash
./sss.php 

```

There is a result in the OpenSIPS:

``` bash

```

### Loop protection 1

``` php
    if(!mf_process_maxfwd_header("10")) {
        sl_send_reply("483","Too Many Hops");
        xlog("L_WARN","$ci|end|Too Many Hops ($fu)");
        exit;
    }
```


### Loop protection 2

``` php
    # Loop protection 2 ????
#    if(is_myself("$rd", "$rp")) {
#       xlog("L_WARN","$ci|end|sourced from this server $rd:$rp");
#       exit;
#    }

```


## Links

* [fail2ban](https://github.com/fail2ban/fail2ban)
* [OpenSIPS](https://opensips.org/)
* [VLAD PAIU,OpenSIPS,Securing SIP Networks](https://opensips.org/pub/events/2012-08-07_ClueCon_Chicago/VLAD_PAIU-OpenSIPS-Securing_SIP_Networks.pdf)