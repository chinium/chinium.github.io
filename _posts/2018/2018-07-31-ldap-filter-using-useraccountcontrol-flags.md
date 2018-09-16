---
layout: post
title: LDAP Filter using userAccountControl flags
date: 2018-07-31 12:00
tags: [LDAP, userAccountControl]
---

이번에는 AD의 userAccountControl flags를 이용해 Disable되어 있지 않은 Normal Account를 Filtering하는 LDAP Filter를 알아보자.

### LDAP Filter
```powershell
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


위 Filter에서
- 2는 ACCOUNTDISABLE (2) - The user account is disabled
- 512는 NORMAL_ACCOUNT - This is a default account type that represents a typical user.
를 의미한다.


자세한 사항은 아래 Reference URLs를 참조하도록 한다.
  

### Ref.
- [https://support.microsoft.com/en-gb/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro](https://support.microsoft.com/en-gb/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro){:target="_blank"}
- [http://www.selfadsi.org/ldap-filter.htm#BitAndOr](http://www.selfadsi.org/ldap-filter.htm#BitAndOr){:target="_blank"}

