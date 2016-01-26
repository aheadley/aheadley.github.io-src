Title: Memcached with Unix Sockets in W3 Total Cache
Date: 2011-11-05 10:01
Author: aheadley
Category: Internet, Linux
Tags: memcached, patch, php, wordpress
Slug: memcache-with-unix-sockets-in-w3-total-cache
Status: published

Wrote a [quick
patch](http://waysaboutstuff.com/files/w3tc-memcache-socket.patch "patch")
to allow using unix socket connections to Memcached for W3 Total Cache
(for a new product line at work):  
[patch gutter="false"]diff -ur
w3-total-cache.orig/lib/W3/Cache/Memcached.php
w3-total-cache/lib/W3/Cache/Memcached.php  
--- w3-total-cache.orig/lib/W3/Cache/Memcached.php 2011-08-26
01:52:28.000000000 -0400  
+++ w3-total-cache/lib/W3/Cache/Memcached.php 2011-11-04
16:47:06.356716709 -0400  
@@ -33,7 +33,13 @@

foreach ((array) \$config['servers'] as \$server) {  
list(\$ip, \$port) = explode(':', \$server);  
- \$this-\>\_memcache-\>addServer(trim(\$ip), (integer) trim(\$port),
\$persistant);  
+ \$ip = trim(\$ip);  
+ \$port = (integer) trim(\$port);  
+ if( @filetype(\$ip) === 'socket' ) {  
+ \$port = 0;  
+ \$ip = 'unix://' . \$ip;  
+ }  
+ \$this-\>\_memcache-\>addServer(\$ip, \$port, \$persistant);  
}  
} else {  
return false;[/patch]

Use by changing to the W3TC plugin directory and running the patch
command with -p1:  
[bash]\$ cd wp-content/plugins/w3-total-cache/  
\$ curl -s http://waysaboutstuff.com/files/w3tc-memcache-socket.patch |
patch -p1[/bash]

Then you can run by setting the memcached server to
"/path/to/socket.sock:0" in W3TC's settings.

I've only tested this against the latest version (0.9.2.4) but it should
probably work against older versions, though I have no idea how far
back. In retrospect, it would probably be better to just check if the
first character of the IP is a '/' (who would use a relative path to a
socket?) to avoid the stat call every time. Oh well.
