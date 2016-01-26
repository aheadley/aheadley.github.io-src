Title: More Things and Stuff
Date: 2012-07-10 23:38
Author: aheadley
Category: Internet, Linux, Tech
Tags: hesperus, interworx, irc, ircbot, packagetrack, python, shellinabox, shipping
Slug: more-things-and-stuff
Status: published

Been busy lately so no updates and what-not. Things I've been working
on:

-   [diskyfm](https://github.com/aheadley/diskyfm "diskyfm on GitHub") -
    A simple python script to wrap mplayer (or any other CLI-based mp3
    player) to make listening to DigitallyImported and Sky.FM stations
    easy to use. Includes bash completion of the station names. As a
    bonus, the code is potentially re-usable, so adding DI/Sky support
    to other stuff could be easy.
-   [packagetrack](https://github.com/aheadley/packagetrack "packagetrack on GitHub") -
    A python library for tracking packages, supports UPS, FedEx, USPS,
    DHL, and CanadaPost. I added the DHL and CanadaPost support and made
    adding additional carriers easier, plus cleaned up the code a lot.
    Still needs some work in the error handling department, especially
    for the older carrier code (UPS and USPS) but is very usable. Kudos
    to the orignal author and the guy who added UPS and USPS support.
-   [shellinabox](http://code.google.com/p/shellinabox/ "shellinabox on Google Code") -
    This is so cool, it lets you embed a very well emulated terminal
    right in the browser. I use it on this site for basic shell access,
    and to provide a gateway to nethack.alt.org through telnet to help
    some friends get around overly restrictive firewalls at work. Also
    working on integrating it with
    [Interworx](http://www.interworx.com/ "Interworx Control Panel") as
    a plugin which should be ready in a few weeks. It's crazy flexible
    and works much better than other similar things I've tried in the
    past (like AjaxTerm). Only bummer (which isn't shellinabox problem)
    is that browsers (or Firefox at least) are not very helpful if you
    try to access a HTTPS service using a self-signed SSL certificate on
    a very high port, like I'm doing with shellinabox in CGI mode for
    the Interworx integration.
-   [hesperus](https://github.com/aheadley/hesperus "hesperus on GitHub") -
    An IRC bot written by one of the developers of Minecraft-Overviewer,
    we use this a lot in the \#overviewer channel. I've added a few
    plugins like one for tracking packages (using packagetrack), and
    another for send text messages to your phone if you are mentioned
    after being idle for a while.
-   [PHP-FPM](http://php.net/manual/en/install.fpm.php "PHP-FPM") - Did
    a lot of work with this so we can switch to it from suPHP at the day
    job. It's pretty cool and will hopefully make a nice improvement in
    server performance.

