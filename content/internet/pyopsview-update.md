Title: pyOpsview Update
Date: 2010-09-16 01:15
Author: aheadley
Category: Internet, Linux
Tags: minecraft, python
Slug: pyopsview-update
Status: published

pyOpsview is pretty much feature complete now I think. I still have some
minor changes to make like adding error catching/handling (it will
pretty much blow up if it runs into anything unexpected right now), some
real documentation since no one is gonna want to read through
help()/docstrings, and maybe some kind of threaded, self-updating node
type. But yeah, you could write code against the current interface and
have it still work with future versions I think. Also, the updated
version of the code in the previous post would be:  
[py autolinks="false"]import opsview  
server =
opsview.OpsviewServer(base\_url='https://example.com/opsview/',
username='opsview\_user', password='opsview\_pass').update()  
print [host['name'] for host in filter(lambda host:
host['max\_check\_attempts'] == host['current\_check\_attempts'],
server.children)]  
server.remote.acknowledge\_all(comment='omg python')[/py]
