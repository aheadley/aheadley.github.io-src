Title: Wordpress get_home_path bug
Date: 2010-03-19 00:41
Author: Alex Headley
Category: Internet
Tags: patch, php, wordpress
Slug: wordpress-get_home_path-bug
Status: published

Spent some time working on a client's site trying to figure out why
Wordpress thought it was installed in '/' (lolwat). Turns out that if
you use http for one part of your site (like the normal pages, the
'home') and https for the admin part (the 'siteurl' part) AND they are
in different paths, get\_home\_path() in wp-admin/includes/file.php
can't figure out where your site is actually located because the
str\_replace() doesn't match. I wrote a quick patch for it, I'm sure
there are better ways of doing it (I've thought of at least 2 just while
writing this) but this gets the job done:  
[diff]\*\*\* wp-admin/includes/file.php 2010-03-19
01:20:34.000000000 -0400  
--- wp-admin/includes/file.php 2010-03-19 01:21:40.000000000 -0400  
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
\*\*\* 66,73 \*\*\*\*  
\* @return unknown  
\*/  
function get\_home\_path() {  
! \$home = get\_option( 'home' );  
! \$siteurl = get\_option( 'siteurl' );  
if ( \$home != '' && \$home != \$siteurl ) {  
\$wp\_path\_rel\_to\_home = str\_replace(\$home, '', \$siteurl); /\*
\$siteurl - \$home \*/  
\$pos = strpos(\$\_SERVER["SCRIPT\_FILENAME"],
\$wp\_path\_rel\_to\_home);  
--- 66,73 ----  
\* @return unknown  
\*/  
function get\_home\_path() {  
! \$home = preg\_replace('/\^.\*?:\\/\\//','',get\_option( 'home' ));  
! \$siteurl = preg\_replace('/\^.\*?:\\/\\//','',get\_option( 'siteurl'
));  
if ( \$home != '' && \$home != \$siteurl ) {  
\$wp\_path\_rel\_to\_home = str\_replace(\$home, '', \$siteurl); /\*
\$siteurl - \$home \*/  
\$pos = strpos(\$\_SERVER["SCRIPT\_FILENAME"],
\$wp\_path\_rel\_to\_home);[/diff]
