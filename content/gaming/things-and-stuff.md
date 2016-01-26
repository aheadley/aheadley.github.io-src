Title: Things and Stuff
Date: 2011-01-10 13:52
Author: aheadley
Category: Gaming, Internet, Linux
Tags: django, github, irc, killing floor, opsview, php, python, world of warcraft
Slug: things-and-stuff
Status: published

So things have been keeping me busy, so I'm going to write about them.
Let's see (brace for github spam):

1.  An [IRC bot](https://github.com/aheadley/python-opsview-ircbot) that
    keeps track of opsview alerts for work, based on the python opsview
    stuff I did earlier. It's kind of crap but it works most of the time
    and I don't use IRC so whatever.
2.  An django-based [imageboard thing](https://github.com/aheadley/dibs)
    originally intended to replace TinyIB since it's crap. Unfortunately
    I got bored so while it probably works, it's completely unstyled and
    basically useless. Oh well, it was mostly an excuse to play with
    Django (which is pretty cool) anyway.
3.  Rewrote the [PHP
    version](https://github.com/aheadley/php-libopsview) of the opsview
    thing so it was less crap and uses Zend\_Http\_Client instead of
    curl since the Zend thing is more elegant or whatever, curl sucks in
    PHP.
4.  Started on a new [Minecraft wrapper script framework
    thing](https://github.com/aheadley/pynecraft) since Flippeh's
    multiplexer (what I'm currently using) is annoying to use for some
    things and just generally has features I don't need (like the whole
    multiplexing thing). Since my interest in Minecraft has waned, so
    has my interest in this, although I am making slow progress in this
    (I guess).

Other things I've been up to are:

Started playing WoW again, thanks to Cataclysm. The new zones are pretty
neat. Although they still lack the, I dunno, unpolished charm some of
the vanilla zones had. I also levelled a worgen up through level 52 or
so. The worgen starting zone is annoying, Blizzard does their typically
thing with a new tech/feature and shoves it down your throat (like
vehicles in WotLK) so there are in-game cinematic things every 5
minutes. I also didn't like how it forces you to travel to the different
parts of the zone rather than naturally leading you around. The quests
were like, "Go here and do these three quests." "Ok, now you're done,
take this horse that's not under your control for a mile to the next
town and do five more quests, then hop the next taxi to the next town,
rinse, repeat." Now that I think about it, this was really present in
all the new zones. The quest designers seem to have gotten a hard-on for
phasing quests which becomes very annoying as it forces the quest
progression to be very linear. You do a handful of quests at the
questhub, then the phase changes which opens the next few quests (maybe
at a new quest hub) and that is repeated until you move to the next
zone. On one hand, it's nice since the player doesn't get overwhelmed
with having 20 quests in their quest log and no idea which ones they
should do first. On the other hand, it definitely gives a railroad-ish
feeling to the whole experience and the switch between the railroad and
non-railroad zones can be kind of jarring (WPL -\> EPL, I'm looking at
you). It was also kind of annoying in the redone vanilla zones to run
into the same quests, but with a different name or design. Westfall was
where I noticed this the most, with almost all of the quests being
identical to the original zone, but with the objectives designed a
little differently (ex: Westfall stew quest was broken into a couple
different quests, generally with easier requirements). On the plus side,
I'm enjoying levelling my human prot warrior and some of the vanilla
instance changes are really nice (like Sunken Temple). BRD comes up way
too often in the LFG queue though. Blizzard needs to make that entire
instance to be around the same level, and make it obvious which one
you're supposed to do since there doesn't seem to be a way to tell when
you join it randomly. If it was all the same level they could shift BRS
down a bit so that it actually gets some use since no one is going to go
there once they hit 57/58.

I picked up Killing Floor  over the Christmas Steam sales since a few
other people from my Minecraft server play it. I haven't had that much
fun playing anything in a while. Nothing beats screaming in vent when a
Fleshpound shows up and starts owning face, and then giggling like a
little girl after everyone has been raped. Not sure how I feel about
playing this now that the Christmas skins are gone though.

I've also been working on a scripting thing for work since right now all
the techs use a mish-mash of custom scripts to do common tasks and we're
trying to standardise to make support easier. The fun part is that it's
in PHP. Yeah. Besides the fact that PHP is a complete hack-job of a
language, it's really just not suited to this sort of thing. Perl would
probably be the best language, but none of us know Perl and we all know
PHP so that settles that. For the most part its not actually that bad,
just every now and then I run into something that makes me facepalm
(like how the last parameter in str\_replace is passed by reference,
WTF).

Anyway, on to the next post.
