Title: PHP PCRE and Unicode Properties
Date: 2011-03-15 19:47
Author: Alex Headley
Category: Linux
Tags: php, regex, unicode
Slug: php-pcre-and-unicode-properties
Status: published

Ran into an interesting issue at work a while back. Most of our servers
are PHP5.2 while a few of the newer ones having PHP5.3. I'm developing a
toolkit for our techs to save time and help standardize on the solutions
for some common issues and when testing it on our PHP5.3 servers, it
didn't work. After some fun debugging, it turns out that a regex would
match against a string on a PHP5.2 server, and wouldn't match the same
string on a PHP5.3 server. It turns out that if you want to use unicode
properties in a PCRE regex, you need to make sure that PHP was **built**
against pcre that was **built** with unicode properties support. Note
that recompiling pcre on the server in question won't do anything, PHP
needs to be rebuilt as well. There's more info and a solution
[here](http://chrisjean.com/2009/01/31/unicode-support-on-centos-52-with-php-and-pcre/).
