---
layout: post
title: Parts of an regular web url
date: 2018-05-12 10:00
tags:
  - url
---



```text
                                                        | request ----------------------------------------------- |
                                                        | path ------------------- |                              |
        | authorization | | domain -------------- |     | directory ---- || file - | | query ---------------- |   |
        |               | |                       |     |                ||        | |                        |   |

https://username:password@www.subdomain.example.com:1234/folder/subfolder/index.html?search=products&sort=false#top

|       |        |        |   |         |       |   |   |       |         |     |    |      |        |    |     |
|       username |        |   |         |       |   |   folder  folder    |     |    |      value    |    value |
protocol         password |   |         |       |   port                  |     |    parameter       parameter  |
                          |   |         |       1st-level-domain          |     file-extension                  fragment
                          |   |         2nd-level-domain                  filename
                          |   3rd-level-domain
                          4th-level-domain
```

