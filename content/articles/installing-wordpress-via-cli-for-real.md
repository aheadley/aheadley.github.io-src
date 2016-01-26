Title: Installing Wordpress via CLI (for real)
Date: 2011-11-02 17:55
Author: Alex Headley
Category: Development
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

    :::php
    define('WP_INSTALLING', true);
    require_once 'wp-load.php';
    require_once 'wp-admin/includes/upgrade.php';
    require_once 'wp-includes/wp-db.php';
    wp_install($blog_title, $admin_username, $admin_email,
    $blog_is_public, $deprecated = '', $admin_password);

Which is awesome, because it's easy to package up in a
[little script](https://github.com/nexcess/wordpress-cli-installer) so
you can actually do the whole install on the CLI. Yay, now automating Wordpress
installs is that much easier!

Note that if you wanted to write your own script to do this, it's
important to note that Wordpress shits the bed if it's not run in the
global scope, i.e. don't put the install process in it's own function
like:

    :::php
    function do_install() {
        define('WP_INSTALLING', true);
        require_once 'wp-load.php';
        require_once 'wp-admin/includes/upgrade.php';
        require_once 'wp-includes/wp-db.php';
        return wp_install($blog_title, $admin_username, $admin_email,
        $blog_is_public, $deprecated = '', $admin_password);
    }
    do_install();

Although it looks like the Wordpress devs are aware of the problem and
are working towards fixing it (though I can't seem to find the relevant
bug report now). There is also this
[work-around](https://gist.github.com/942539), though I haven't
tried it. It's also a good idea to define `WP_SITEURL` as the base URL
for the blog before doing the install because otherwise Wordpress can't
figure it out and you'd have to go back and update the database directly
to fix it after the install.
