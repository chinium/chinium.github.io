---
layout: post
title: figlet - ASCII Text Banner
date: '2018-04-08 03:00'
tags:
  - Linux
  - figlet
  - ASCII
published: true
categories: IT
---


FIGlet - display large characters made up of ordinary screen characters  
URL: <http://www.figlet.org/>{:target="_blank"}

### install
```bash
[root@my-dev temp]# yum install figlet
```


### example
```bash
[root@my-dev temp]# figlet Hello world
 _   _      _ _                            _     _
| | | | ___| | | ___   __      _____  _ __| | __| |
| |_| |/ _ \ | |/ _ \  \ \ /\ / / _ \| '__| |/ _` |
|  _  |  __/ | | (_) |  \ V  V / (_) | |  | | (_| |
|_| |_|\___|_|_|\___/    \_/\_/ \___/|_|  |_|\__,_|

[root@my-dev temp]# figlet -ptk -f shadow "Hello World"
 |   |        |  |          \ \        /             |      |
 |   |   _ \  |  |   _ \     \ \  \   /  _ \    __|  |   _` |
 ___ |   __/  |  |  (   |     \ \  \ /  (   |  |     |  (   |
_|  _| \___| _| _| \___/       \_/\_/  \___/  _|    _| \__,_|

[root@my-dev temp]# figlet -f lean "Hello World" | tr ' _/' ' ()'


    ()    ()            ()  ()
   ()    ()    ()()    ()  ()    ()()
  ()()()()  ()()()()  ()  ()  ()    ()
 ()    ()  ()        ()  ()  ()    ()
()    ()    ()()()  ()  ()    ()()



  ()          ()                      ()        ()
 ()          ()    ()()    ()  ()()  ()    ()()()
()    ()    ()  ()    ()  ()()      ()  ()    ()
 ()  ()  ()    ()    ()  ()        ()  ()    ()
  ()  ()        ()()    ()        ()    ()()()


```
