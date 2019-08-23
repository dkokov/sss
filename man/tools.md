# PHP SSS tools

The main php script is 'sss.php'.
Can be started as bash script(have to execute 'chmod u+x sss.php' before).

```bash
# ./sss.php 
  Help:
    -f  fname   	send message from text file 'fname'
    -ka 		send noSIP message for keepalive
    	loop    	send noSIP messages for keepalive - flooding
    -reg		send SIP REGISTER message , normal format'
    	erase   	send SIP REGISTER message with 'Contact: *' and 'expire: 0'
    	fault   	send SIP REGISTER message with errors as headers or headers content
    	fuzzer  	send SIP REGISTER message with syntax/grammar errors	-inv
```

In this script has some included test cases,but you can be created your own test case.


