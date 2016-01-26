Title: More Opsview API love...
Date: 2010-09-09 10:07
Author: Alex Headley
Category: Development
Tags: minecraft, opsview, python, linux
Slug: more-opsview-api-love
Status: published

Oh Opsview, how I hate thee, let me count the ways... For something I
hate, you'd think I'd spend less (or no) time working on shit to
interact with it and yet, here I am writing a [Python interface for its
API](http://github.com/aheadley/pyopsview)

This is actually about 20 times cooler than the PHP version (that I
haven't worked on in months), allow me to demonstrate:

    :::python
    #!/usr/bin/python
    import opsview
    server = opsview.OpsviewServer(
        remote=opsview.OpsviewRemote('opsview.domain.com', 'opsview_user', 'opsview_pass'))
    print [host['name'] for host in filter(
        lambda host: host['max_check_attempts'] == host['current_check_attempts'], server.hosts)]
    server.remote.acknowledgeAll(comment='omg python')

Holy shit, I just output all the servers that are currently down and
then acknowledged all servers and services currently alerting in 4 lines
of code, goddamn! Now I just need to actually do something useful with
this and I'll be in fucking business, ~~maybe I should write some
documentation too~~ (haha, no)! I haven't had a chance to test some
(most) of the functionality though, mainly the stuff that actually sends
XML to opsview like clone/create/reload/etc, need to get a VMware
appliance setup for testing or something.

Oh yeah, and I made some [map generator](http://github.com/aheadley/pynemap)
thing since Cartograph blows on Linux

P.S. the Opsview API still blows.
