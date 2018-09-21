---
layout: post
title: Python - daemonize package
date: 2014-09-02 12:00
tags:
  - Python
  - daemonize
---


## Installation
```bash
(dev)[root@localhost python]# pip install daemonize
'Downloading/unpacking daemonize
  Downloading daemonize-2.3.1.tar.gz
  Running setup.py egg_info for package daemonize

Installing collected packages: daemonize
  Running setup.py install for daemonize

Successfully installed daemonize
Cleaning up...
```

## Help
```bash
(dev)[root@localhost daemonize]# python
Python 2.7.2 (default, Jul  1 2011, 21:32:15)
[GCC 4.1.2 20080704 (Red Hat 4.1.2-48)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import daemonize
>>> dir(daemonize)
['Daemonize', '__builtins__', '__doc__', '__file__', '__name__', '__package__', 'atexit', 'fcntl', 'grp', 'handlers', 'logging', 'os', 'pwd', 'resource', 'signal', 'sys']
>>> dir(daemonize.Daemonize)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'exit', 'sigterm', 'start']
>>> help(daemonize.Daemonize)
Help on class Daemonize in module daemonize:

class Daemonize(__builtin__.object)
 |  Daemonize object
 |  Object constructor expects three arguments:
 |  - app: contains the application name which will be sent to syslog.
 |  - pid: path to the pidfile.
 |  - action: your custom function which will be executed after daemonization.
 |  - keep_fds: optional list of fds which should not be closed.
 |  - auto_close_fds: optional parameter to not close opened fds.
 |  - privileged_action: action that will be executed before drop privileges if user or
 |                       group parameter is provided.
 |                       If you want to transfer anything from privileged_action to action, such as
 |                       opened privileged file descriptor, you should return it from
 |                       privileged_action function and catch it inside action function.
 |  - user: drop privileges to this user if provided.
 |  - group: drop privileges to this group if provided.
 |  - verbose: send debug messages to logger if provided.
 |  - logger: use this logger object instead of creating new one, if provided.
 |
 |  Methods defined here:
 |
 |  __init__(self, app, pid, action, keep_fds=None, auto_close_fds=True, privileged_action=None, user=None, group=None, verbose=False, logger=None)
...
```

## Example
{% gist 45431c3c153d8422415ef4df0c2e6cb6 %}


## Run
```bash
(dev)[root@localhost daemonize]# python daemonize_ex1.py
(dev)[root@localhost daemonize]# ps -ef | grep daemonize
root     18370     1  0 23:56 ?        00:00:00 python daemonize_ex1.py
root     18403 14147  0 23:57 pts/0    00:00:00 grep daemonize
(dev)[root@localhost daemonize]# tail -f test.log
2014-09-01 23:56:59+0900 proc start
2014-09-01 23:57:02+0900 proc loop
2014-09-01 23:57:05+0900 proc loop
2014-09-01 23:57:08+0900 proc loop
^C
(dev)[root@localhost daemonize]# cat test.pid
18370(dev)[root@localhost daemonize]# kill -TERM `cat test.pid`
(dev)[root@localhost daemonize]# ps -ef | grep daemonize
root     18427 14147  0 23:57 pts/0    00:00:00 grep daemonize
```
