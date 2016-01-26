Title: Steam on a Network Share 2: Electric Boogaloo
Date: 2012-02-03 11:53
Author: Alex Headley
Category: Gaming, Linux, Windows
Tags: fedora, iscsi, lvm, ntfs, steam
Slug: steam-on-a-network-share-2-electric-boogaloo
Status: published

Unfortunately, the NTFS junctions in the [previous
post](http://waysaboutstuff.com/blog/archives/226 "Steam on a Network Share")
didn't actually end up working that well. I ran into more of the same
issues with the Steam buildbot failing on games with install processes.
After spending some time looking at other solutions, I decided to give
iSCSI a try. I knew a little about it but had avoided it previously
since it can't (easily) be shared between multiple machines at the same
time and it probably wouldn't work with Wine at all, since Wine has
issues with NTFS filesystems, but nothing else I could find was better.

Fortunately iSCSI works out very well! Setting it up on the server side
was as easy as creating a new logical volume with:  
[bash gutter="false"]lvcreate -L \${SIZE}G -n \$NAME
\$VOLUME\_GROUP[/bash]  
Then installing and starting the iSCSI target (server) tools:  
[bash gutter="false"]yum install -y scsi-target-utils  
chkconfig tgtd on  
service tgtd start[/bash]  
A quick note about the scsi target utils: it seems that this particular
implementation is relatively new and was only added to the mainline
kernel in 3.0 or 3.1 so if you were using an earlier kernel you'd need
to use the set of patches or something. There are also a couple other
iscsi target implementations but I didn't have much luck with getting
them installed since they seemed to be limited to pre-3.0 kernels.

Now we can create our LUN with:  
[bash gutter="false"]tgtadm --lld iscsi --op new --mode
logicalunit --tid 1 --lun 1 -b \$LOGICAL\_VOLUME  
tgtadm --lld iscsi --op bind --mode target --tid 1 -I ALL[/bash]  
Note that this gives full access to the device to anyone that connects
to it so limiting access with firewall rules is probably a good idea.
There is some authentication support but I didn't try it. Of course, you
don't have to use a LV, you can use any old block device and I think it
would even work with just a regular file.

At this point you can switch to Windows and connect using Microsoft's
iSCSI Initiator (client) by going to Control Panel \> Administrative
Tools \> iSCSI Initiator . Then just enter the target's IP
address/hostname and hit connect. The new disk then acts like a locally
connected drive and you can partition, format, and assign a drive
letter.

Since it acts just like a regular local disk, Steam and everything else
seems to work fine. Initially performance was pretty terrible for me
(like 10-20MB/s read speeds) but I tracked that down to a hardware issue
on the server and once it was fixed I was able to saturate the gigE
connection (\~100MB/s).
