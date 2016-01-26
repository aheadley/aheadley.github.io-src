Title: PHP and Opsview
Date: 2010-08-01 03:00
Author: aheadley
Category: Internet, Linux
Tags: googlecode, opsview, php
Slug: lib-opsview-api
Status: published

Like OMG is this exciting or what? I started working on a
class/library/interface/whatever to talk to an opsview server in PHP:  
[lib-opsview-api](http://code.google.com/p/lib-opsview-api/)

It's pretty neat and will let do all kinds of really awesome things (not
really) like make a bot that automatically logs into servers and
restarts apache when a server alerts (and then notifies me via IM or
something?). It's also full of all kinds of super-duper enterprise class
code (\*and\* it uses XML, whoo!) like this:  
[php]  
\$alerting = array();  
//TODO: this is wrong, need to change either here or filters in
getStatusAll()  
\$alerting\_raw = \$this-&gt;getStatusAll(array(  
\$this-\>states['critical'],  
\$this-\>states['warning'],  
\$this-\>states['unhandled'],  
));

switch (\$this-\>config['content\_type']) {  
/\* lol whoops, wasn't thinking ahead on this. since the hostname is  
\* is used as the array key if there is more than one service alerting  
\* but unacknowledged only the last service will actually be
acknowledged  
\* since the others get overwritten  
\* TODO: fix this, use arrays for services maybe?  
\*/  
case 'xml':  
\$alerting\_raw =
simplexml\_load\_string(\$alerting)-\>opsview-\>data-\>list;[/php]  
SO AWESOME!

As a side note, the opsview API is terrible and stupid and bad and is
missing like half the things it should have.
