---
layout: post
title: VBScript - Check & Add UserAccount on Windows
date: 2014-09-30 12:00
tags: [VBScript, Window User Account]
categories: [VBScript]
---

## Using WMI Service
* GetObject("winmgmts:\\ ...
* Win32_UserAccount WMI Class  
  <http://msdn.microsoft.com/en-us/library/aa394507(v=vs.85).aspx>{:target="_blank"}


## Using ADSI (Active Directory Service Interfaces)
* <http://msdn.microsoft.com/en-us/library/aa746512(v=vs.85).aspx>{:target="_blank"}


## Example
{% gist 63d78ec7f8ec74e5f8801ae117f864e1 %}

