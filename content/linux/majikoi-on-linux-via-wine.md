Title: MajiKoi on Linux via Wine
Date: 2011-10-28 22:41
Author: aheadley
Category: Linux, Visual Novel, Wine
Tags: fedora, majikoi
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

[bash]sudo mount -t iso9660 -o utf8,ro path/to/Disc1.mdf /mnt/[/bash]

I was never able to get the installer to recognize the discs, so I ended
up copying all the files out of the ISOs to the program directory with:

[bash]mkdir \~/.wine/drive\_c/Program\\ Files\\ \\(x86\\)/MajiKoi && \\  
while ! sudo rsync -vxhr --progress /mnt/ \~/.wine/drive\_c/Program\  
Files\\ \\(x86\\)/MajiKoi/; do  
echo "Copy didn't finish successfully, trying again..."; sleep 1;  
done[/bash]

Do the same thing for the second disc. Then we need to fix ownership and
permissions:

[bash]sudo chown -R user.user \~/.wine/drive\_c/Program\\ Files\  
\\(x86\\)/MajiKoi/ && \\  
find \~/.wine/drive\_c/Program\\ Files\\ \\(x86\\)/MajiKoi -type
f -print0 | xargs -0 chmod 640 && \\  
find \~/.wine/drive\_c/Program\\ Files\\ \\(x86\\)/MajiKoi -type
d -o -type f -name '\*.exe' -print0 | xargs -0 chmod 750[/bash]

Once that's done, we can go ahead and apply the translation patch (note
that the installation program was skipped, I was never able to get it to
fully work):

[bash]wget
'http://yandere.gray7.com/Majikoi\_Momoyo\_English\_%5BYandere\_Translations%5D.zip'
&& \\  
unzip Majikoi\_Momoyo\_English\_\\[Yandere\_Translations\\].zip -o -d
\~/.wine/drive\_c/Program\\ Files\\ \\(x86\\)/MajiKoi/ && \\  
rm -f Majikoi\_Momoyo\_English\_\\[Yandere\_Translations\\].zip[/bash]

Then replace \~/.wine/drive\_c/Program\\ Files\  
\\(x86\\)/MajiKoi/reg.dll with the cracked version (from the torrent),
otherwise you'll get messages about the DRM crap when you try to run the
game.

Finally, we can run the game now:

[bash]LC\_ALL=ja\_JP.utf8 LANG=ja\_JP.utf8 wine
\~/.wine/drive\_c/Program\\ Files\\ \\(x86\\)/MajiKoi/Majikoi.exe[/bash]

[![](http://waysaboutstuff.com/blog/wp-content/uploads/2011-10-28-233045_1919x1079_scrot-600x337.png)](http://waysaboutstuff.com/blog/archives/100/2011-10-28-233045_1919x1079_scrot)

As for how well Wine runs the game itself, it's worked fine as far as
I've gotten. The only real issues are that a few of the scene
transitions/CG effects and the UI lag kind of bad (particularly the
effect when you're sick in Chris's route). I also ran into a crash with
a message about the MS VC++ runtime at the end of the prologue but I
just restarted the game and didn't see it again after reloading that
save.
