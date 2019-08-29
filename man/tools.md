# PHP SSS tools

  The idea is very simple - copy and paste really SIP messages and to use as prototypes/models
in the SIP test cases.It releases with PHP scripts.

The **PHP SSS tools** packet is content follow files:

* **sss.php**      - a main PHP script(exec);
* **config.php**   - any definitions and params;
* **lib.sss.php**  - all PHP functions;
* subdir **tmpl**  - a set of really SIP messages as files(prototypes/models);


The '**sss.php**' can start as bash script (have to execute '**chmod u+x sss.php**' before).
When start the script without options,will see the follow display(help):

```bash
./sss.php
Help :

./sss.php
		-f  	fname                    	*send message from text file "fname" with options or without
		    	        empty            	*send and recv,waiting only one response
		    	        loop             	*send and recv in loop mode,stop/interept script with <Ctrl> + <C>
		    	        flood            	*send with max speed,without recv waiting in loop mode,stop/interept script with <Ctrl> + <C>
		    	                fuzz     	*send message from the file with syntax/grammar errors

		-ka 	                         	*send noSIP message for keepalive

		-reg
		    	empty                    	*send and recv,waiting only one response
		    	loop                     	*send and recv in loop mode,stop/interept script with <Ctrl> + <C>
		    	flood                    	*send with max speed,without recv waiting in loop mode,stop/interept script with <Ctrl> + <C>
		    	        empty            	*send SIP REGISTER message
		    	        erase            	*send SIP REGISTER message with 'Contact: *' and 'expire: 0'
		    	        fault            	*send SIP REGISTER message with errors as headers or headers content
		    	        fuze             	*send SIP REGISTER message with syntax/grammar errors

		-inv

```

In this script has some included test cases,but you can be created your own test case.


## Copy-Paste SIP message as template

  In the subdir '**tmpl**' has a set of really SIP messages.The PHP script '**lib.sss.php**' uses for to generate messages.
You can be used this feature for your own test case.Sould be get message from somebody really communication...best is from your lab(your local network).
Copy and paste this message in a file.After that start follow:

``` bash
./sss.php -f your_file_name
```

It's very easy,isn't ?


## SIP REGISTER test cases

