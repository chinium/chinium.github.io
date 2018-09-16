---
layout: post
title: Run Length Encoding
date: 2018-04-13 12:00
tags: [Compression, Encoding, Algorithm]
---


### Run Length Encoding ###

- 데이터의 반복성을 이용하는 가장 간단한 압축 기법
- Run 은 반복되는 문자, Length 는 반복되는 횟수를 의미
    - AAAABBBBBCCCCCCCCDEEEE (22 byte)  
      => 4A5B8C1D4E (10 byte)  
      압축률 : 22 byte / 10 byte = 2.2  
    - MyDogHasFleas (13 byte)  
      => 1M1y1D1o1g1H1a1s1F1l1e1a1s (26 byte)  
      압축률 : 13 byte / 26 byte = 0.5 (원시 자료보다 2배 증가)  
      (반복된 데이터에 적용하지 않으면 비효율적임)
- 보완방법: 반복되지 않은 데이터의 경우 원데이터를 표현하고, Run length encoding은 반복된 데이터에 적용
    - 전치문자 + 데이터의 개수 + 원데이터의 해당문자
    - ABCDDDDDDDDEEEEEEEEE ( 19 byte)  
      => ABC*8D*9E ( 9 byte)  
      압축률 : 19 byte / 9 byte = 2.11  
      3 byte보다 긴 run 일 경우 압축률이 높아짐  
