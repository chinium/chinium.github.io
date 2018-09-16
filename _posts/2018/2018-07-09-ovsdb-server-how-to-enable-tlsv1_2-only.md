---
layout: post
title: ovsdb-server - How to enable tlsv1_2 only
date: 2018-07-09 12:00
tags:
  - oVirt
  - ovsdb-server
  - tls
---


### ovsdb-server (Port 6641, 6642)

**- Port 6641 (OVN_Northbound)**

```bash
# SELECT
[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnnb_db.sock '["OVN_Northbound", {"op":"select", "table":"SSL", "where":[]}]' | python -m json.tool
[
    {
        "rows": [
            {
                "_uuid": [
                    "uuid",
                    "07061153-385c-4f1c-b983-eede6a532c49"
                ],
...snip...
                "ssl_ciphers": "",
                "ssl_protocols": ""
            }
        ]
    }
]

# UPDATE
[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnnb_db.sock '["OVN_Northbound", {"op":"update", "table":"SSL", "where": [["_uuid","==",["uuid","07061153-385c-4f1c-b983-eede6a532c49"]]], "row": {"ssl_protocols": "TLSv1.2"}}]' | python -m json.tool
[
    {
        "count": 1
    }
]

[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnnb_db.sock '["OVN_Northbound", {"op":"select", "table":"SSL", "where":[]}]' | python -m json.tool
[
    {
        "rows": [
            {
...snip...
                "ssl_ciphers": "",
                "ssl_protocols": "TLSv1.2"
            }
        ]
    }
]
```


**- Port 6642 (OVN_Southbound)**

```bash
# SELECT
[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnsb_db.sock '["OVN_Southbound", {"op":"select", "table":"SSL", "where": []}]' | python -m json.tool
[
    {
        "rows": [
            {
                "_uuid": [
                    "uuid",
                    "b375d965-3d62-4536-af48-143ead738612"
                ],
...snip...
                "ssl_ciphers": "",
                "ssl_protocols": ""
            }
        ]
    }
]

# UPDATE
[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnsb_db.sock '["OVN_Southbound", {"op":"update", "table":"SSL", "where":[], "row":{"ssl_protocols": "TLSv1.2"}}]' | python -m json.tool
[
    {
        "count": 1
    }
]

# SELECT
[root@my-dev ~]# ovsdb-client transact unix:/var/run/openvswitch/ovnsb_db.sock '["OVN_Southbound", {"op":"select", "table":"SSL", "where": []}]' | python -m json.tool
[
    {
        "rows": [
            {
...snip...
                "ssl_ciphers": "",
                "ssl_protocols": "TLSv1.2"
            }
        ]
    }
]
```


- Ref.
	- [https://relaxdiego.com/2014/09/ovsdb.html](https://relaxdiego.com/2014/09/ovsdb.html){:target="_blank"}
	- [https://tools.ietf.org/id/draft-pfaff-ovsdb-proto-02.html#rfc.section.5.2.3](https://tools.ietf.org/id/draft-pfaff-ovsdb-proto-02.html#rfc.section.5.2.3){:target="_blank"}
	- [https://bugzilla.redhat.com/show_bug.cgi?id=1459441](https://bugzilla.redhat.com/show_bug.cgi?id=1459441){:target="_blank"}
	- [http://docs.openvswitch.org/en/latest/ref/ovsdb-server.7/](http://docs.openvswitch.org/en/latest/ref/ovsdb-server.7/){:target="_blank"}

