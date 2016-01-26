Title: SecuROM is the Cancer Killing the PC Gaming Industry
Date: 2012-02-18 21:36
Author: aheadley
Category: Gaming, Rant, Windows
Tags: digital distribution, drm, iscsi, piracy, securom, steam
Slug: securom-is-the-cancer-killing-the-pc-gaming-industry
Status: published

**DISCLAIMER:** Pretty much everything in this post has probably already
been said (and said better) elsewhere on the internet, I just feel like
ranting after dealing with DRM for 3 days.**  
**

I like gaming. Not as much as I used to, but every now and then I want
to fire up a game of Homeworld or Crysis or whatever. What I don't want
to have to do is search through my big binder of discs for the game and
hope it still works/isn't scratched/lost/etc. Clearly a better solution
is needed, but I didn't want to turn to no-cd cracks and other
questionably safe/legal methods if possible. I mean, I have the
official, store-bought discs, surely I can figure some way to do this...
right?

*Note: This part is somewhat technical, the tl;dr is that a neat
solution to a stupid problem doesn't work because of DRM.* The rough
plan when I started out was "use STGT iSCSI target to emulate DVD drive
backed by ISO images of the game discs". Unfortunately, it seems STGT
uses non-standard ISO images with some extra metadata added so I
wouldn't be able to do this without making special copies from the real
physical disc to the special ISO images mounted in STGT which didn't
seem like a great idea. So the next idea was to emulate a real DVD drive
on the target, then use STGT's pass through to deliver that to the
initiator. CDemu worked to emulate the DVD drive (and is a pretty neat
piece of software) although it was somewhat of a pain to install on
Fedora. Unfortunately it seems STGT's pass through capability is either
incompatible with the MS WIndows initiator or just incomplete or broken
as this never quite worked. Windows would hang whenever the pass through
drive was attached and tgtd would spew errors on the server. The last
thing I could think of was to try a different target implementation so I
switched to SCST (IET seemed to be too old to work from the last time I
was looking at iSCSI targets and LIO seemed extremely complicated). SCST
configuration documentation is lacking but I eventually got it working
and Windows saw the emulated drive, things seemed to be working great.
Unfortunately (I am using that word a lot in this post), I ran into a
brick wall called SecuROM. After many hours of trying different things,
I got Crysis (and only Crysis) to work with the iSCSI drive using
SecuROM's diagnostic tool. None of the other SecuROM-protected games
worked and as far as I can tell, there is no way to get them working
without either getting a full no-cd patch or using a program to hide the
non-iSCSI drives from SecuROM. I'm not interested in either solution so
I guess I'll just be sticking to the original discs for those games.
After wasting about 3 days trying to setup a work-around for the
retarded fallout of paranoid publishing companies (and failing), all I
can say is *"FUCK YOU SECUROM"*.

Of course, this is really the fault of the companies that insist on
ineffective strategies to try and turn pirates into paying customers.
Right now, you can ignore the obvious free/non-free difference and the
pirates are still offering the better product. Let me say that again
because it is truly mind-boggling: *the pirated game could cost more
than retail and still be the better product*. No DRM, easy to use, no
worries about lost/damaged discs, and you don't even have to get off
your ass to get it. This is why Steam is destroying it's competition;
they fucking *get it*. Ease of use, low price, reliability, un-obtrusive
DRM, it's all there with Steam. If they started adding music/movies/TV
episodes and seasons with a sane pricing model and the same kind of
sales that the games get, well... I don't know how many olympic-sized
swimming pools GabeN would be able to fill with money, but I would wager
more than a few. Now that's not to say that Steam can do no wrong, just
that they do so much more right than everyone else. I wish Steam would
go back and start adding older titles to their catalog (like Homeworld,
Black & White, etc). Their sales can also get kind of stale ("Oh boy,
\$5 off that \$60 game for the 8th time in a row!") or just suck
sometimes ("50% off all Train Simulator DLC, who gives a shit!?"), but
those are really just minor issues.

At the end of the day, the fact of the matter is that I (as a paying
customer) have been frustrated by DRM and pirates that got the game for
free have a better experience. Well fuck that, if the companies are
going to treat their customers like shit, I just won't buy (or play)
their games anymore. Naturally, the sales loss from decisions like mine
will be attributed to piracy, and you know what? They'd be completely
correct, the pirates are eating their fucking lunch because they offer
better service.
