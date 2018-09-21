---
layout: post
title: vagrant box prune / remove
date: '2018-04-10 12:00'
tags:
  - vagrant
published: true
categories: IT
---


이번에는 vagrant box subcommand 중에 prune과 remove의 차이를 살펴보자.

각각의 사용법은 다음과 같다.

```bash
mac-sample:~ chinium$ vagrant box
Usage: vagrant box <subcommand> [<args>]

Available subcommands:
     add
     list
     outdated
     prune
     remove
     repackage
     update

For help on any individual subcommand run `vagrant box <subcommand> -h`

mac-sample:~ chinium$ vagrant box prune -h
Usage: vagrant box prune [options]

Options:

    -p, --provider PROVIDER          The specific provider type for the boxes to destroy.
    -n, --dry-run                    Only print the boxes that would be removed.
        --name NAME                  The specific box name to check for outdated versions.
    -f, --force                      Destroy without confirmation even when box is in use.
    -h, --help                       Print this help

mac-sample:~ chinium$ vagrant box remove -h
Usage: vagrant box remove <name>

Options:

    -f, --force                      Remove without confirmation.
        --provider PROVIDER          The specific provider type for the box to remove
        --box-version VERSION        The specific version of the box to remove
        --all                        Remove all available versions of the box
    -h, --help                       Print this help
```


실제 사용해보자.

vagrant box prune을 사용하면 older box를 체크해 제거한다.

```bash
mac-sample:~ chinium$ vagrant box list
bento/centos-7.2    (virtualbox, 2.3.0)
bento/centos-7.2    (virtualbox, 2.3.1)
centos/7            (virtualbox, 1801.02)
centos/7            (virtualbox, 1802.01)

mac-sample:~ chinium$ vagrant box prune --name centos/7
The following boxes will be kept...
centos/7            (virtualbox, 1802.01)

Checking for older boxes...
Removing box 'centos/7' (v1801.02) with provider 'virtualbox'...

mac-sample:~ chinium$ vagrant box list
bento/centos-7.2    (virtualbox, 2.3.0)
bento/centos-7.2    (virtualbox, 2.3.1)
centos/7            (virtualbox, 1802.01)
```


vagrant box remove는 지정한 이름의 box를 제거하거나 box 버전까지 지정해서 제거한다.

```bash
mac-sample:~ chinium$ ls -al .vagrant.d/boxes/bento-VAGRANTSLASH-centos-7.2/2.3.0
total 0
drwxr-xr-x  3 chinium  staff   96 Oct 28  2016 .
drwxr-xr-x  5 chinium  staff  160 Dec  6  2016 ..
drwxr-xr-x  6 chinium  staff  192 Oct 28  2016 virtualbox

mac-sample:~ chinium$ vagrant box remove bento/centos-7.2 --box-version 2.3.0
Removing box 'bento/centos-7.2' (v2.3.0) with provider 'virtualbox'...

mac-sample:~ chinium$ vagrant box list
bento/centos-7.2    (virtualbox, 2.3.1)
centos/7            (virtualbox, 1802.01)

mac-sample:~ chinium$ ls -al .vagrant.d/boxes/bento-VAGRANTSLASH-centos-7.2/2.3.0
total 0
drwxr-xr-x  2 chinium  staff   64 Apr 10 12:13 .
drwxr-xr-x  5 chinium  staff  160 Dec  6  2016 ..
```
