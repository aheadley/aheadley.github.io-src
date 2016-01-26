Title: Steam on a Network Share
Date: 2011-11-12 19:28
Author: aheadley
Category: Gaming, Windows
Tags: nfs, ntfs, samba, smb, steam
Slug: steam-on-a-network-share
Status: published

Steam is many things, some good, some bad; but what it isn't is light on
disk space usage when you have a lot of games (especially modern AAA
titles). One way to deal with this is to simply buy bigger disks, but
that would be too easy (and expensive with the recent spike in prices)
so I decided to stick all my steam games on my file server. First, let
me explain what led up to this:

1.  I recently upgraded my home network to GigE and it's just so damn
    fast, everything should be over the network now!
2.  I have some vague notion of being able to play my games in both
    WIndows and Wine from the same set of files. This is probably never
    going to happen but it would be cool.
3.  My fileserver has 4TB of free disk space, I'll be damned if it's
    just going to sit around doing nothing.
4.  Windows is installed on a slow-as-shit PATA drive on my desktop.

Which naturally led to the idea of putting Steam (and other
miscellaneous games) on the file server and sharing them with my
desktop. Brilliant, right? Now, about my setup so that these examples
make sense: my fileserver is at 10.0.0.11 and exports /storage (the 7TB
XFS LV) on both NFS and Samba (named "storage"), and I'm putting all the
game stuff in /storage/wine/drive\_d (for some reason that is lost to me
now).

So of course, the first step is copying everything to the fileserver,
which is trivial and so I'm not going to cover it (if you can't figure
it out this isn't for you). Next, we need to share all these game files.
I know Windows supports SMB/Samba and I already have it setup, but I'd
recently heard mention that there is some NFS client for Windows and I
figure that NFS will probably give better performance so let's go with
that! **Don't**, just don't. NFS on Windows is a fucking disaster. If
you were very familiar with Windows then maybe you could get it working
reliably but, well, I'm not a Windows sysadmin. It would sometimes work
after a reboot, and sometimes the permissons in the share would be wrong
(everything read-only), and sometimes it just wouldn't work at all. So
forget that, and move on to SMB.

I already "knew of" (had heard mentioned once) NTFS Junctions which are
like symlinks for Windows! Except not really but maybe sometimes if you
squint just right and it's raining and a full moon (more on this in a
bit). So I installed Steam to "C:\\Program Files (x86)\\Steam" (like
normal), then mapped the Samba share as a network drive (X:\\ because
Z:\\ never worked again after the NFS disaster). Then I deleted Steam's
"steamapps" directory (since this is where it stores all the game data
is what actually takes up all that disk space) and make a link with
(make sure this is a prompt with Administrator priviledges):  
[bash gutter="false"]\> mklink /D "C:\\Program Files
(x86)\\Steam\\steamapps" "X:\\wine\\drive\_d\\steamapps"[/bash]  
This actually sort of works and I was able to play DX:HR and
Battlefield 2 and a couple other games. Unfortunately it fails
completely for most games, specifically it looks like any game that has
a install/setup process (which is run by the Steam buildbot or something
like that). The problem is that NTFS "symlinks" (junctions) are retarded
and only work in some situations and not at all in others, so a symlink
is visible when used in some ways, and is apparently completely
invisible when used in others (possibly related to being an argument for
a command, which I seriously hope I'm wrong about). I guess this makes
sense if you're ~~brain damaged~~ a NTFS developer? Anyway, this causes
the buildbot to just die when it tries to access stuff in the steamapps
directory so those games never get through the install process and so
are unplayable. Awesome.

I almost called it quits here, I was so frustrated. Never mind the fact
that symlinks are awesome and incredibly useful and I can't believe that
NTFS only kind of half-way supports them but don't tell anyone man or
else they might actually try to use them. Actually, no, that's pretty
much the whole issue. Whiskey Tango Foxtrot. Handling them (or not) in
this way is just... I don't know how to express how dumbfounded I am by
the stupid restrictions and counter-intuitiveness they have. However, I
did not in fact give up, and instead thought "hey, what if I just
mounted the SMB share somewhere under C:\\ ?", since that might let me
work-around the can't-symlink-to-network-shares bullshit. But I wasn't
actually able to find a way to do that, apparently it can only be done
with local volumes (it's actually impressive how wrong Windows gets
filesystems and mounting them); what I did find was that you **can**
actually symlink to network shares, as long as you do it in a way that
is completely non-obvious. You see, the *mklink* command earlier was
close, but not quite stupid enough, what I actually needed was:  
[bash gutter="false"]\> mklink /D "C:\\Program Files
(x86)\\Steam\\steamapps"
"\\\\10.0.0.11\\storage\\wine\\drive\_d\\steamapps"[/bash]  
Lo and behold everything fucking works now. Why the already mapped
network drive doesn't work I will probably never know (and I'm not sure
I want to). Performance is nothing to write home about, but not terrible
either. At this point I'm tired of messing with it but I'll probably see
if I can tweak some SMB settings or something in the future because I'm
not even come close to maxing the GigE link (which is much lower that
the max read disk read speed).
