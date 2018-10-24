---
layout: post
title: ovsdb-server - How to enable tlsv1_2 only
date: 2018-07-09 12:00
tags:
  - oVirt
  - ovsdb-server
  - tls
categories:
  - IT
---

여기서는 ovbdb-server에서 TLSv1.2만 접속 허용하도록 설정하는 방법을 알아보도록 한다.


## ovsdb-server (Port 6641, 6642)

{% gist 0d29fe19adc72f43032a099b37809789 %}


## Ref.
- [https://relaxdiego.com/2014/09/ovsdb.html](https://relaxdiego.com/2014/09/ovsdb.html){:target="_blank"}
- [https://tools.ietf.org/id/draft-pfaff-ovsdb-proto-02.html#rfc.section.5.2.3](https://tools.ietf.org/id/draft-pfaff-ovsdb-proto-02.html#rfc.section.5.2.3){:target="_blank"}
- [https://bugzilla.redhat.com/show_bug.cgi?id=1459441](https://bugzilla.redhat.com/show_bug.cgi?id=1459441){:target="_blank"}
- [http://docs.openvswitch.org/en/latest/ref/ovsdb-server.7/](http://docs.openvswitch.org/en/latest/ref/ovsdb-server.7/){:target="_blank"}

