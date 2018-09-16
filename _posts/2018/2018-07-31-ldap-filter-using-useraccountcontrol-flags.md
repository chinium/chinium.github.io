---
layout: post
title: LDAP Filter using userAccountControl flags
date: 2018-07-31 12:00
tags: [LDAP, userAccountControl]
---


```vbnet
(&
  (objectClass=user)
    (objectCategory=person)
    (mail=*)
    (userAccountControl:1.2.840.113556.1.4.803:=512)
    (!
        (userAccountControl:1.2.840.113556.1.4.803:=2)
    )
)
```


- Ref.
	- [https://support.microsoft.com/en-gb/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro](https://support.microsoft.com/en-gb/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro){:target="_blank"}
	- [http://www.selfadsi.org/ldap-filter.htm#BitAndOr](http://www.selfadsi.org/ldap-filter.htm#BitAndOr){:target="_blank"}

