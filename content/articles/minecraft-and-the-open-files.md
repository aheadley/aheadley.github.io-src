Title: Minecraft and the Open Files
Date: 2011-03-18 19:07
Author: Alex Headley
Category: Gaming
Tags: minecraft, multiplayer, notch, linux
Slug: minecraft-and-the-open-files
Status: published

Discovered an interesting bug in the SMP server: it doesn't close file
handles for regions, ever. The tl;dr version is that by default (at
least on CentOS) for large maps (\>900 regions) you will eventually run
out of file handles and Minecraft will crash because it can't re-open
the session file, because the default open file limit is 1024.

The best part is that the same region file can get opened multiple
times, using up even more file handles (probably caused by players
leaving the region's area then returning)! This should help demonstrate
("underwater\_base" is the name my world and PID 748 was the Minecraft
server at the time):

    :::bash
    find underwater\_base/region/ -type f -name 'r.*.*.mcr' | wc -l
    1739
    ls -l server.log.lck
    -rw-r--r-- 1 thedaily thedaily 0 Mar 17 21:00 server.log.lck
    lsof -p 748 | grep REG | wc -l
    630
    lsof -p 748 | grep REG | awk '{print \$9}' | sort | uniq -c | sort -nr | head
    8 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-34.8.mcr
    7 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-34.7.mcr
    7 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-3.-1.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-8.-1.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-31.8.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-31.7.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-2.-1.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-2.0.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-12.-1.mcr
    6 /chroot/home/thedaily/thedailyautist.com/minecraft/underwater\_base/region/r.-1.-1.mcr[/bash]

Yep, awesome. Fortunately there is a relatively simple work around via
ulimit:

    :::bash
    ulimit -n 4096

Unfortunately that will fail by default on CentOS, you also need to
raise the hard limit in /etc/security/limits.conf by adding a line
like:

    $username hard nofile 4096

Where $username is the username you run the Minecraft server under.
Then stop your Minecraft server, log out, log back in, run the above
ulimit command, then you can safely re-start the server with the new
higher limit. Or rather, you'll be safe until you have \~4K regions, at
which point you'll want to do this again with an even higher number.

**Edit:** Five days later, that same process is still running:

    :::bash
    lsof -p 748 | wc -l
    2857
