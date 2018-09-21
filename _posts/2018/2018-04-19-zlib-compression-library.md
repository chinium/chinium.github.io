---
layout: post
title: zlib Compression Library
date: '2018-04-19 10:00'
Tags:
  - Compression
  - zlib
published: true
categories: IT
---


## zlib
- 일반적인 용도로 법적 제약 없이 자유롭게 사용가능한 무손실 데이터 압축 라이브러리
- Jean-loup Gailly (compression)와 Mark Adler (decompression)에 의해 작성됨
  (Jean-loup는 gzip의 주요 제작자이기도 하며, Mark 또한 gzip의 제작자이자 UnZip의 메인 압축 해제 루틴을 작성했으며 Zip의 원작자다. zlib에 사용된 압축 알고리즘은 gzip, Zip과 근본적으로 동일함)
- LZW 압축 방식과 달리 zlib에서 사용되는 압축 방식에서는 데이터를 확장시키지 않음
  (LZW는 심한 경우 파일 크기를 2~3배까지 커지게 함)
- zlib는 libpng의 필수적인 부분이며 PNG를 지원하는 많은 애플리케이션에서 다양하게 테스트되었음


### Ref.
- <http://zlib.net/>{:target="_blank"}
