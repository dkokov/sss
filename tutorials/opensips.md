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

Really this server can do a lot.
My expirence with OpenSIPS is starting before seven years.
I was in Chicago on ClueCon2012.I have included in OpenSIPS traing then.
I have seen VLAD's presentation by a conference.
It was my first OpenSIPS security release.

Let's go to see follow topology: The OpenSIPS as Load Balancer with SBC functionality.
The Load Balancing will be into several FreeSWITCHes.
The FreeSWITCH keeps media streams and release voice services,also same SIP registrations.
The registrations are only passing along a OpenSIPS.

Test cases are release with **OpenSIPS** _version 2.4.5_ and **FreeSWITCH** _version 1.6.20_

## Features

* [Malformed packets](#malformed-packets)
* [Loop protections](#loop-protections)

* [Flooding protection with PIKE](flooding-protection-with-pike)

* [Check 'UA'](#check-ua)
* [Check 'From' URI](#check-from-uri)
* [Check 'To' URI](#check-to-uri)
* [List from unavailable SIP messages](list-from-unavailable-sip-messages) 
* [Counter 'no auth'](#counter-no-auth)
* [Check REGISTRATION](#check-registration)
* [INVITE prevent](#invite-prevent)
* [REGISTER prevent](#register-prevent)

* [fail2ban using](#fail2ban-using)
* [compression and logrotate a opensips log](#compression-and-logrotate-a-opensips-log)

### Malformed packets 

For this task,you can be used a follow OpenSIPS's function (according to SIP RFC3261):

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

### Loop protections

For this task,you can be used a follow OpenSIPS's function:

* [is_maxfwd_lt()](https://opensips.org/html/docs/modules/2.4.x/maxfwd.html#func_is_maxfwd_lt)


``` php
???
    if(!mf_process_maxfwd_header("10")&& $retcode==-1) {
        sl_send_reply("483","Too Many Hops");
        xlog("L_WARN","$ci|end|Too Many Hops ($fu)");
        exit;
    }

???
    # next hope is a gateway, so make no sens to
    # forward if MF is 0 (after decrement)
    if(is_maxfwd_lt("1")) {
	sl_send_reply("483","Too Many Hops");
	exit;
    };

```


``` php
    # Loop protection 2 ????
#    if(is_myself("$rd", "$rp")) {
#       xlog("L_WARN","$ci|end|sourced from this server $rd:$rp");
#       exit;
#    }

```

### Flooding protection with PIKE

PIKE is sample DoS protection OpenSIPS module.

``` php

```
opensipsctl fifo pike_list


### Check UA

Can be written in '**route[chk_ua]**'.


### Check 'From' URI

Can be written in '**route[chk_from_uri]**'.

### Check 'To' URI

Can be written in '**route[chk_to_uri]**'.


### List from unavailable SIP messages 

If you want to stop some messages,shold used a function 'is_method()' for to recognize the same _**METHOD**_.
After that to return "**Service Unavailable**" response for example.
Maybe would like to stop 'OPTIONS' or "MESSAGE" again.

``` php
    if(is_method("PUBLISH|SUBSCRIBE")) {
        sl_send_reply("503","Service Unavailable");
        exit;
    }
```

### Counter 'no auth'

If you would like to count attempts...
The idea for this 'route' is gotten by VLAD's presentation.
For to use in different places in the config script,
you can be eritten as route

There is a same route:

``` php
route[no_auth_counter]
{
    if(cache_fetch("redis:group1","authF_$fU",$avp(failed_no))) {
        if($(avp(failed_no){s.int}) > 6) {
            return($(avp(failed_no){s.int}));
        } else {
            if($param(1)==1) $avp(failed_no_curr) = $(avp(failed_no){s.int}) + 1;
            else $avp(failed_no_curr) = $(avp(failed_no){s.int});
        }
    } else {
        $avp(failed_no_curr) = 0;
    }

    if($var(d)) xlog("L_INFO","$ci|debug(no_auth_counter($param(1)))|src_ip: $si,authF: $fU,counter: $avp(failed_no_curr)");
    if($param(1)==1) cache_store("redis:group1","authF_$fU","$avp(failed_no_curr)",300);

    return(-1);
}
```

### INVITE prevent

### REGISTER prevent

### 'fail2ban' using

  Very importment is to use '**fail2ban**' because all prevention mechanisms here,will write in log key word 'reject'.
Something have to get this key word and start droping for this ip address. There will stop attacks !
You can be used '**fail2ban**' for this task.

For to work '**OpenSIPS**' and '**fail2ban**' together,in the '**opensips.cfg**' have to use follow conception:

``` php
xlog("L_INFO", "$ci|reject|src_ip=[$si],...your message description...");
```

The follow message will be write in the opensips log file:
``` bash
1118856626301574502321685|reject|src_ip=[192.168.108.180],... your message description ...
```

There is a syntax for '**fail2ban**' rule:

``` bash
failregex = \|reject\|src_ip=\[<HOST>\]
```

See more information for fail2ban in the [**fail2ban tutarial**](fail2ban.md)


### compression and logrotate a opensips log

## Links

* [OpenSIPS](https://opensips.org/)
* [VLAD PAIU,OpenSIPS,Securing SIP Networks](https://opensips.org/pub/events/2012-08-07_ClueCon_Chicago/VLAD_PAIU-OpenSIPS-Securing_SIP_Networks.pdf)
* [PIKE](https://opensips.org/html/docs/modules/2.4.x/pike.html)


