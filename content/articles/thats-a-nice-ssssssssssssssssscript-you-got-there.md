Title: That's a nice ssssssssssssssssscript you got there...
Date: 2010-08-10 06:20
Author: Alex Headley
Category: Internet, Linux
Tags: minecraft, python
Slug: thats-a-nice-ssssssssssssssssscript-you-got-there
Status: published

http://code.google.com/p/minecraftadmin/﻿

So I've been contributing code to this Python script for admining
Minecraft SMP alpha servers. It is pretty neat, though it can't (ever)
do some of the things the various Java wrappers can, I like it because
it's easy to modify and stuff on the fly and doesn't have to be compiled
or anything stupid like that.

Sometime down the road (a.k.a. never) I'd love to rewrite it into a
proper module/class and clean up the code so that you could do something
like  
[python]import Minebot  
svr = Minebot('/path/to/minecraft\_server.jar')  
svr.start()  
svr.registerEvent('player\_login', lambda player: svr.give(player, 1,
'stone'))  
svr.say('Let the games begin!')[/python]  
or something, I'm not very creative (I also have no idea if the that
would even work). The point is it'd be super easy to run multiple
servers from a single pair of script and server.jar files (not that this
is really all that important) and then you could setup some kind of
neato hosting service with a fancy web interface and stuff.
