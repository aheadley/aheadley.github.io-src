Title: Installing Wordpress via CLI (for real)
Date: 2011-11-02 17:55
Author: aheadley
Category: Internet, Linux
Tags: cli, github, php, wordpress
Slug: installing-wordpress-via-cli-for-real
Status: published

It would seem that most (all) of the instructions found on Google for
installing Wordpress via the CLI go through the steps of downloading and
extracting the Wordpress package, then end with "and now open your site
in the browser and finish the installation"... kind of misses the point,
yeah. So I started looking through the normal web install script and it
looks like the actual meat of the installation (assuming you've already
filled out wp-config.php) is just a few short lines:  
[php]define('WP\_INSTALLING', true);  
require\_once 'wp-load.php';  
require\_once 'wp-admin/includes/upgrade.php';  
require\_once 'wp-includes/wp-db.php';  
wp\_install(\$blog\_title, \$admin\_username, \$admin\_email,  
\$blog\_is\_public, \$deprecated = '', \$admin\_password);[/php]

Which is awesome, because it's easy to package up in a little script so
you can actually do the whole install on the CLI. Which I've done (for
work)
[here](https://github.com/nexcess/wordpress-cli-installer "GitHub").
Yay, now automating Wordpress installs is that much easier!

Note that if you wanted to write your own script to do this, it's
important to note that Wordpress shits the bed if it's not run in the
global scope, i.e. don't put the install process in it's own function
like:  
[php]function do\_install() {  
define('WP\_INSTALLING', true);  
require\_once 'wp-load.php';  
require\_once 'wp-admin/includes/upgrade.php';  
require\_once 'wp-includes/wp-db.php';  
return wp\_install(\$blog\_title, \$admin\_username, \$admin\_email,  
\$blog\_is\_public, \$deprecated = '', \$admin\_password);  
}  
do\_install();[/php]  
Although it looks like the Wordpress devs are aware of the problem and
are working towards fixing it (though I can't seem to find the relevant
bug report now). There is also this
[work-around](https://gist.github.com/942539 "GitHub"), though I haven't
tried it. It's also a good idea to define 'WP\_SITEURL' as the base URL
for the blog before doing the install because otherwise Wordpress can't
figure it out and you'd have to go back and update the database directly
to fix it after the install.
