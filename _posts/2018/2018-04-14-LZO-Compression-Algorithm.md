---
layout: post
title: LZO Compression Algorithm
date: 2018-04-14 10:00
tags: [Compression, Algorithm, LZO]
---


## LZO (Lempel-Ziv-Oberhumer)
- Markus Oberhumer가 1994년에 ANSI C로 개발한 무손실 데이터 압축 알고리즘
- 압축 효율, 압축 해제 속도에 초점을 둔 알고리즘 (zlib, bzip에 비해 거의 5배 이상 빠름)
- 리눅스 커널, 안드로이드 모바일 기기, 임베디드 디바이스 및 OpenVPN, MPlayer2, Libav, FFmpeg 같은 오픈소스 라이브러리 등에 널리 사용되고 있음
- 사용자가 압축 효율과 속도간 밸런스를 조정할 수 있으며, 압축 해제 속도에는 영향을 미치지 않음
- GNU General Public License (GPL v2+)에 따라 배포 가능
- **miniLZO**
    - very lightweight subset of the LZO library.
    - 하나의 C 소스 파일과 3개의 헤더 파일로 구성
- **LZOP**
    - 압축 서비스를 위해 LZO를 이용해 만든 file compressor로, gzip과 유사함
    - gzip에 비해 높은 압축/압축 해제 속도가 특징


### Related Links
- <http://www.oberhumer.com/opensource/lzo/>{:target="_blank"}
- <https://en.wikipedia.org/wiki/Lempel–Ziv–Oberhumer>{:target="_blank"}
- [zlib library](http://www.zlib.net/){:target="_blank"}
- [gzip file compressor](http://www.gzip.org/){:target="_blank"}
- [bzip2 file compressor](http://www.bzip.org/){:target="_blank"}
- [xz file compressor](https://tukaani.org/xz/){:target="_blank"}


### Example - miniLZO
{% highlight bash %}
[root@my-dev temp]# wget http://www.oberhumer.com/opensource/lzo/download/minilzo-2.10.tar.gz -O- | tar xvfpz -
--2018-04-14 06:38:52--  http://www.oberhumer.com/opensource/lzo/download/minilzo-2.10.tar.gz
Resolving www.oberhumer.com (www.oberhumer.com)... 193.170.194.40
Connecting to www.oberhumer.com (www.oberhumer.com)|193.170.194.40|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 63314 (62K) [application/x-gzip]
Saving to: ‘STDOUT’
...snip...
100%[=============================================================================================================>] 63,314      73.0KB/s   in 0.8s
[root@my-dev temp]# cd minilzo-2.10 && ls -al
total 396
drwxr-xr-x. 2 root root    141 Mar  1  2017 .
drwxr-xr-x. 3 root root     26 Apr 14 06:38 ..
-rw-r--r--. 1 root root  18092 Mar  1  2017 COPYING
-rw-r--r--. 1 root root  16006 Mar  1  2017 lzoconf.h
-rw-r--r--. 1 root root 127289 Mar  1  2017 lzodefs.h
-rw-r--r--. 1 root root   2042 Mar  1  2017 Makefile
-rw-r--r--. 1 root root 213674 Mar  1  2017 minilzo.c
-rw-r--r--. 1 root root   3089 Mar  1  2017 minilzo.h
-rw-r--r--. 1 root root   4003 Mar  1  2017 README.LZO
-rw-r--r--. 1 root root   4675 Mar  1  2017 testmini.c

[root@my-dev temp]# cat README.LZO
...snip...
Appendix B: list of public functions available in miniLZO
 ---------------------------------------------------------
 Library initialization
    lzo_init()

 Compression
    lzo1x_1_compress()

 Decompression
    lzo1x_decompress()
    lzo1x_decompress_safe()

 Checksum functions
    lzo_adler32()

 Version functions
    lzo_version()
    lzo_version_string()
    lzo_version_date()

 Portable (but slow) string functions
    lzo_memcmp()
    lzo_memcpy()
    lzo_memmove()
    lzo_memset()
...snip...

[root@my-dev minilzo-2.10]# make

Welcome to miniLZO. Please choose one of the following 'make' targets:

    gcc:   gcc
    unix:  hpux hpux9
    win32: win32-bc win32-cygwin win32-dm win32-lccwin32
           win32-intelc win32-mingw win32-vc win32-watcomc
    dos32: dos32-djgpp2 dos32-wc

[root@my-dev minilzo-2.10]# make gcc
gcc -I. -I../include/lzo -s -Wall -O2 -fomit-frame-pointer -o testmini testmini.c minilzo.c
[root@my-dev minilzo-2.10]# ./testmini

LZO real-time data compression library (v2.10, Mar 01 2017).
Copyright (C) 1996-2017 Markus Franz Xaver Johannes Oberhumer
All Rights Reserved.

compressed 131072 bytes into 593 bytes
decompressed 593 bytes back into 131072 bytes

miniLZO simple compression test passed.
{% endhighlight %}
