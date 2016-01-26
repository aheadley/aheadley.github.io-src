Title: Why I hate the Minecraft
Date: 2011-03-15 19:38
Author: aheadley
Category: Gaming, Internet, Rant
Tags: minecraft, multiplayer, notch
Slug: why-i-hate-the-minecraft
Status: published

So I've been running a public SMP Minecraft server since the day Notch
added multiplayer and I feel like venting. Brace for nerdrage.

-   ﻿**Notch is a terrible programmer.** I don't mean that he can't
    write code, or that Minecraft is a bad game. Clearly neither of
    those are true. The issue is that Notch seems to have no idea about
    how to manage an actual project instead of some hobby toy. At the
    beginning of SMP, most updates broke about as much (or more) as they
    added, Things that worked fine would get broken and Notch would
    actually not know what was wrong or how to fix them. Leaf decay was
    the worst example but definitely not the only one. This along with
    the twits about trying git or Subversion suggest that he hadn't been
    using some kind of VCS for a while there. I'm not suggesting that
    anyone who doesn't use Subversion is terrible, especially not right
    at the beginning of a project when you would be commiting huge
    changes that would make no sense to revert. But when you're
    accepting money from people for something and don't take even the
    most basic of steps to fix your own screw-ups... I don't know what
    to say. I would be fucking ashamed of myself. Then there's that
    whole singleplayer and multiplayer use different code-bases so every
    feature you add gets to be coded twice (double the bugs at no extra
    charge!) thing, this should probably be pretty high priority to
    reduce the work load in the future but I guess when you take 3 week
    vacations every other month you just don't have time for fixing real
    problems. The vacations bit wouldn't even be that big a deal if it
    didn't feel like he was practically bragging about them all over the
    only real official news sources for the game, his blog and twitter
    account. It's hard to imagine that he can't see how players go to
    either or both of those hoping for some news on the game they enjoy
    so much and instead see "LOL JUST GOT BACK FROM GDC, GOING TO VEGAS
    NOW" or other inane drivel that gets posted on Twitter and not feel
    like they were cheated out of their \$15 bet on future updates. It
    just boggles my mind how utterly unprofessional he treats the whole
    thing. Getting back on track here, the SMP server software is
    technically multithreaded, but in practice only a single thread is
    actually doing all the real work. This is a massive problem mostly
    because it completely kills any benefit of
    multi-core/multi-processor systems (you know, like pretty much every
    single modern server) since the current trend is more cores at a
    lower clock speed. This means there is basically a hardware enforced
    limit to the number of players a server can support, and you will
    very quickly run into the end of your hardware upgrade path because
    Intel and AMD are not really competing over gigahertz anymore, there
    simply won't be a faster CPU to get, and there is nothing you can do
    about it. The SMP server also hits the disk, a lot. Like it was
    competing with a heavily loaded database server for who could do
    more random reads. This was helped significantly by the new region
    format, but the fact that the region format was lifted from the
    MCRegion mod nearly unaltered speaks volumes. Not because a mod was
    incorporated into the game, no (that was actually great), but that
    they did it instead of fixing the real problems with the chunk
    format. That mostly being that the NBT files are gzipped, which is
    all kinds of stupid ("Let's add even more overhead to each read
    operation to an app that is already read intensive!"). There's also
    the fact that it seems like there's approximately zero testing done
    before a release. I'm not saying I expect bug free software, it's in
    beta after all. I'm talking about, why in the flipping fuck would
    you push out an update that has major game-breaking
    shit-obviously-doesn't-fucking-work bugs? Oh yeah, and the update is
    mandatory! Fuck you Notch, maybe you could try actually playing this
    game you're being paid to develop for 5 minutes before forcing an
    update on everyone. Or not, I guess that *would* really be the
    I'm-not-a-giant-prick thing to do. Oh yeah, and when does SMP get
    the nether? You know, from that whole HUGE HALLOWEEN UDPATE? I guess
    I missed the "except for SMP players" fine print. It's amazing that
    Bukkit got the Nether added before the people WITH THE ACTUAL
    NON-OBSFUCATED SOURCE CODE TO THE FUCKING SERVER SOFTWARE. Guess
    Notch was too busy adding beds that don't even work. I'd also like
    the official mod API we were supposed to get back in like December,
    so I wouldn't have to deal with the stupid inter-mod incompatibility
    crap.
-   **The SMP server software is shit.** This is partly an extension of
    the first one, but one I used to be able to mostly forgive since
    it's quite possible that Notch didn't have any experience running a
    server before. *Used to*, now it's 6+ months later and very little
    has changed. How is the server shit? Let me count the ways: 1) When
    the server crashes (as in SEVERE exception thrown) it often doesn't
    really crash, just happily chugs along doing absolutely nothing, so
    I get to wake up in the morning and find out the servers been down
    for 6 hours. 2) The server maintains a session lock file on the
    world you start it on. But you say "that doesn't sound so bad..." oh
    yeah, and it checks that session file for updates every second... in
    an already disk intensive app. Why does Notch think it's the
    server's responsibility to stop the admin from being an idiot and
    running two servers on the same world? If you do something that
    stupid, you deserve the world corruption you get. 3) The bullshit
    "Did the time change or is the server overloaded?" message gets
    logged. Over and over. This is a minor nitpick and trivial to work
    around, but still annoying. 4) stdin and stdout/stderr are not
    separated. This one is hard for me to describe exactly, but when
    you're typing in the console and the server prints something to
    stdout it hides whatever was being typed. Bukkit finally added a
    better console to recent versions but the fact that they needed to
    is fairly retarded.
-   **The community is retarded.** No, I'm being serious here. I mean
    these people are pants-on-head,
    how-do-they-manage-to-walk-and-breath-at-the-same-time retarded. Go
    to the [Minecraft
    Forums](http://www.minecraftforum.net/viewforum.php?f=10) and read
    the first 5 threads. If 4 out of 5 aren't something like "how i port
    4wd on my comcast" I would be shocked. *Shocked.*Sure, parts of the
    community are pretty cool and contribute really awesome tools like
    Minecraft-Overviewer, pigmap, c10t, and the various server mods, but
    then you have the rest of it full of mind-numbing stupidity. This
    also extends to the actual players as well, of course. I've met some
    cool players, but there's so many assholes and retards that I
    sometimes wonder why I even fucking bother with this shit.

In conclusion, I hate this game and don't know why I play it or run a
server. Also, fuck Notch.
