Title: MajiKoi on Linux via Wine
Date: 2011-10-28 22:41
Author: Alex Headley
Category: Linux
Tags: fedora, majikoi, wine, gaming
Slug: majikoi-on-linux-via-wine
Status: published

So with an animu currently airing and a (slow) translation project
underway, /a/ has been flipping out about MajiKoi and got me interested
in trying the game to see what all the fuss is about (the first episode
being a total "WTF am I watching/this is pretty awesome"-fest helped).
But since just installing on Windows would be too easy, I decided to try
it on my laptop with Fedora 15 and Wine. Here are the steps I took to
get it working.

First, you need to get a hold of a copy of the game along with the
cracked reg.dll file. The popular torrent floating around has both. Once
downloaded, we mount it with:

    :::bash
    sudo mount -t iso9660 -o utf8,ro path/to/Disc1.mdf /mnt/

I was never able to get the installer to recognize the discs, so I ended
up copying all the files out of the ISOs to the program directory with:

    :::bash
    # set install dir so we don't keep typing it
    MK_INSTALL_DIR=~/.wine/drive_c/Program\ Files\ \(x86\)/MajiKoi/
    mkdir $MK_INSTALL_DIR && \
    while ~ sudo rsync -rvxh --progress /mnt/ $MK_INSTALL_DIR; do \
        echo "Copy didn't finish successfully, trying again..."; sleep 1;
    done

Do the same thing for the second disc. Then we need to fix ownership and
permissions:

    :::bash
    sudo chown -R user.user $MK_INSTALL_DIR && \
    find $MK_INSTALL_DIR -type f -print0 | xargs -0 chmod 644 && \
    find $MK_INSTALL_DIR -type f -iname '*.exe' -print0 | xargs -0 chmod 755 && \
    find $MK_INSTALL_DIR -type d -print0 | xargs -0 chmod 755

Once that's done, we can go ahead and apply the translation patch (note
that the installation program was skipped, I was never able to get it to
fully work):

    :::bash
    wget 'http://yandere.gray7.com/Majikoi_Momoyo_English_%5BYandere_Translations%5D.zip' && \
    unzip 'Majikoi_Momoyo_English_[Yandere_Translations].zip' -o -d $MK_INSTALL_DIR && \
    rm -f 'Majikoi_Momoyo_English_[Yandere_Translations].zip'

Then replace `$MK_INSTALL_DIR/reg.dll` with the cracked version (from the torrent),
otherwise you'll get messages about the DRM crap when you try to run the
game.

Finally, we can run the game now:

    :::bash
    LC_ALL=ja_JP.utf8 LANG=ja_JP.utf8 wine $MK_INSTALL_DIR/Majikoi.exe

[![](http://waysaboutstuff.com/blog/wp-content/uploads/2011-10-28-233045_1919x1079_scrot-600x337.png)](http://waysaboutstuff.com/blog/archives/100/2011-10-28-233045_1919x1079_scrot)

As for how well Wine runs the game itself, it's worked fine as far as
I've gotten. The only real issues are that a few of the scene
transitions/CG effects and the UI lag kind of bad (particularly the
effect when you're sick in Chris's route). I also ran into a crash with
a message about the MS VC++ runtime at the end of the prologue but I
just restarted the game and didn't see it again after reloading that
save.
