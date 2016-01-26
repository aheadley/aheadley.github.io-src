Title: Extending LVM on Software RAID
Date: 2012-01-06 21:33
Author: Alex Headley
Category: Linux
Tags: fedora, lvm, mdadm, raid
Slug: extending-lvm-on-software-raid
Status: published

I added some new disks to my fileserver recently, and wanted to add them
to my sw raid5 arrays, then add the extra space to the LVM volume group
and the only logical volume in it. Unfortunately there is very little
information out there about doing this, but it is possible, and it's
pretty much as straight forward as it would seem. For my example, we'll
say the new disk is /dev/sdd (formatted with one big partition), the sw
raid is /dev/md0 with 3 disks already, and the logical volume is lv01 in
VG vg00. First we need to add the new disk to the array:

    :::bash
    mdadm /dev/md0 -a /dev/sdd1

That will add it as a spare, then we'll grow the array to use the new
disk:

    :::bash
    mdadm --grow /dev/md0 -n 4

This reshape will take a while (hours) to run, depending on the size of
the array and the speed of your machine and disks and controller. You
can check the progress with:

    :::bash
    cat /proc/mdstat

Once it's finished, we need to tell LVM about the new space by first
increasing the size of the Physical Volume (PV) on /dev/md0:

    :::bash
    pvresize /dev/md0

Now the Volume Group (VG) knows about the extra space but it hasn't
been assigned to a Logical Volume (LV) so we do that:

    :::bash
    lvresize -l 100%FREE /dev/vg00/lv01

The last step is to increase the size of the filesystem on top of the
logical volume. It looks like there is an option for lvresize to do this
but I haven't used it so I don't know if it works or not. Depending on
the filesystem you're using it may need to be unmounted before resizing
it (EXTx does, XFS doesn't) and the tool used to do the resizing will
also depend on the filesystem so consult google for that. For XFS it was
just:

    :::bash
    xfs_growfs /path/to/mountpoint
