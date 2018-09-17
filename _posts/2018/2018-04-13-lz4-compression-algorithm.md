---
layout: post
title: LZ4 Compression Algorithm
date: 2018-04-13 10:00
tags: [Compression, Algorithm, LZ4]
published: true
---


### LZ4
- compression, decompression speed에 초점을 둔 무손실 데이터 압축 알고리즘 (lossless data compression algorithm)
    - compression speed at 400 MB/s per core (0.16 Bytes/cycle)
    - decompression speed in multiple GB/s per core (0.71 Bytes/cycle)
- LZ77 계열 (byte-oriented compression schemes)
- LZO 알고리즘보다 약간 떨어지는 compression ratio
- A high compression derivative, called LZ4_HC
- open source software using a BSD license
- <http://lz4.github.io/lz4/>{:target="_blank"}
- <https://en.wikipedia.org/wiki/LZ4_(compression_algorithm)>{:target="_blank"}

