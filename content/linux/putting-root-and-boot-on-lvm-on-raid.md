Title: Putting root and /boot on LVM on RAID
Date: 2012-02-24 15:54
Author: aheadley
Category: Linux
Tags: fedora, grub, lvm, mdadm, raid
Slug: putting-root-and-boot-on-lvm-on-raid
Status: published

As a holdover from its original install on Fedora 14 or 15, my server
had the main system partitions (`/`, `/boot`, `swap`) on md raid across
2 80GB disks (`sda` and `sdk`) while all my media is on LVM backed by
RAID5. In a fit of madness, I recently decided to switch those system
partitions to LVM as well, also backed by RAID5 (with an additional 80GB
drive [`sde`]). Amazingly this worked without a hitch (and only 2
reboots) but I'm going to document my steps just in case someone else
finds them useful. Also, I'll get to the details later but GRUB2 is
required, old GRUB will not work. Another note is that while there is
lots of documentation saying that GRUB2 can boot of LVM **or** MD raid I
couldn't find anywhere that said if you could combine them (LVM on RAID)
but as this shows, you can and it works just fine.

First, let me give more details on the original setup. The system
partitions in the old fstab looked something like this:  
[plain gutter="false"]/dev/md0 / ext4 \#the rest of the space  
/dev/md1 /boot ext4 \#512MB  
/dev/md2 /tmp ext4 \#2GB  
/dev/md3 swap swap \#4GB[/plain]  
md0 through md2 were RAID1 partitions and md3 was RAID0. While I was
doing this I also decided I wanted `/home` and `/var` to be their own
volumes and `/tmp` should become a tmpfs like `/dev/shm` Hopefully that
is enough info to make sense of the rest of this.

The first thing I did was create the new LV (logical volume) for `/home`
(called *lv\_home*) on my big fast VG (volume group) that primarily
holds my media (called *vg\_data*), then formatted and rsync'd the live
`/home` to the new `/home` and swapped them out:  
[bash]lvcreate -L 50G -n lv\_home vg\_data \#create the LV  
mkfs.ext4 /dev/vg\_data/lv\_home \#format it  
mkdir /mnt/tmp  
mount /dev/vg\_data/lv\_home /mnt/tmp/  
rsync -avx /home/ /mnt/tmp/ \#sync it with live  
\#swap it out for live  
mv /home{,.orig} && mkdir /home && \\  
mount /dev/vg\_data/lv\_home /home/ && rm -rf /home.orig[/bash]  
Now you'll want to update `/etc/fstab` to point to the new `/home` (if
you use UUIDs you can get the new one with `blkid`). I rebooted at this
point because I needed to remove a custom kernel that I was running at
the time (for iSCSI stuff in the last post) but if it's not needed for
this LVM stuff.

Now before we get into this mess we need to install grub to the disk
(`sdk`) in the system RAID1 that we'll keep until the end in case things
go south, and remove swap completely so that this recovery environment
can boot cleanly:  
[bash]grub2-install /dev/sdk  
swapoff /dev/md3 \#turn swap off  
mdadm --stop /dev/md3 \#stop the swap disk  
\#edit /etc/fstab to remove the swap line  
\#edit /etc/mdadm.conf to remove /dev/md3  
\#remove the sda partitions from /dev/md[012]  
mdadm --zero-superblock /dev/sda? \#wipe all md data from sda[/bash]

Now we format the two disks that will form our (degraded) RAID5
partition to use as the PV (physical volume) in the new system VG
(*vg\_system*), create the new system LVs and sync them with the live
partitions. Note that I use GPT partitions instead of MBR, MBR would
probably work but I wanted to be consistent with my other disks:  
[bash]gdisk -l /dev/sda \#partition table after formatting  
Number Start (sector) End (sector) Size Code Name  
1 2048 6143 2.0 MiB EF02 BIOS boot partition  
2 10240 156215085 74.5 GiB FD00 Linux RAID  
[/bash]  
There are a couple of things to note about that partition table:

-   The 2MB BIOS Boot partition is where grub will install it's extra
    modules so that we can use both LVM and RAID
-   I stick a few MBs of space between partitions and at the end of disk
    to account for the slight sector count differences between disks
    that are the same "size"

[bash]sgdisk -G -R /dev/sde /dev/sda \#copy the partition table from sda
to sde  
partprobe \#make sure the new partition tables are visible  
mdadm --create /dev/md127 -n 3 -l 5 /dev/sd[ae]2 missing \#create the
raid5 partition  
pvcreate /dev/md127  
vgcreate vg\_system /dev/md127  
lvcreate -L 512M -n lv\_boot vg\_system  
\#repeat lvcreate for /, /var, and swap  
mkswap /dev/vg\_system/lv\_swap \#create swap space  
mkfs.ext4 /dev/vg\_system/lv\_{root,boot,var} \#make new
filesystems[/bash]  
Now you'll need to mount the new root, boot, and var LVs and sync them
with their live counterparts (using `rsync`). After that we can get
everything ready to boot from the new LVs:  
[bash]\#setup mount points for chroot  
mount /dev/vg\_system/lv\_root /mnt/tmp  
mount /dev/vg\_system/lv\_boot /mnt/tmp/boot  
mount /dev/vg\_system/lv\_var /mnt/tmp/var  
mount -o bind /home /mnt/tmp/home  
mount -o bind /dev /mnt/tmp/dev  
mount -t proc none /mnt/tmp/proc  
chroot /mnt/tmp/ /bin/bash \#chroot into the new root[/bash]  
From here, we need to edit `/etc/mdadm.conf` to add the new RAID5 disk
we created and remove the old RAID1 disks we don't want anymore. You can
see what the new entry should look like by running
`mdadm --detail --scan`. After that, fix `/etc/fstab` to point at the
new LV-based partitions, and I added the tmpfs entry for `/tmp`. Then we
can tell grub to load the right modules by adding a line to
`/etc/default/grub` like this:  
[plain gutter="false"]GRUB\_PROLOAD\_MODULES="raid mdraid1x
lvm"[/plain]  
Order may be important there, I didn't test to find out. Now we just
need to finish a couple things in the chroot and we'll be ready to
reboot onto the new LVs:  
[bash]grub2-mkconfig -o /boot/grub2/grub.cfg \#rebuild the grub config  
\#you can probably ignore the errors it spits out about LVM  
mv /boot/initramfs-\$(uname -r).img{,.orig}  
dracut /boot/initramfs-\$(uname -r).img \$(uname -r) \#make new initrd  
grub2-install /dev/sda[/bash]  
Now exit the chroot, reboot, and hope everything works! If the machine
comes back up and is on the new LVs we can go ahead and finish cleaning
up after the old partitions:  
[bash]\# use mdadm --stop to stop any of the old arrays that were
automatically started  
sgdisk -G -R /dev/sdk /dev/sde \#give sdk the new partition table  
partprobe \#make it visible  
mdadm /dev/md127 -a /dev/sdk2 \#add sdk to the new RAID5 which will
start rebuilding  
\#install grub to the other system disks so any of them can be used if
sda fails  
grub2-install /dev/sde  
grub2-install /dev/sdk[/bash]  
And that's it, enjoy your new LVM backed system partitions!
