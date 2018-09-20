---
published: true
title: Jekyll과 File System Events
slug: jekyll-and-file-system-events
date: '2018-09-20 23:49'
tags:
  - Jekyll
  - File System Events
categories:
  - IT
---

## Jekyll 사이트 미리보기시 옵션

Jekyll을 로컬에서 미리보기 용도로 사이트를 띄울 때 지정하는 옵션 중에 -w, --watch가 있는데,
이는 파일시스템의 변화를 감지해서 사이트를 리빌드하도록 할 때 사용된다.

```
$ jekyll help serve
jekyll serve -- Serve your site locally

Usage:

  jekyll serve [options]

Options:
...snip...

        -w, --[no-]watch   Watch for changes and rebuild
        
...snip...
```


## 파일 시스템 이벤트(File System Events, FSEvents)

파일 시스템 이벤트는 Mac에서 특정 디렉토리 계층의 콘텐츠가 변경되었을 때 통지받는 데 이용되는 이벤트를 말한다. 예를 들어, 특정 애플리케이션에서 특정 디렉토리에 속한 파일들이 변경되었을 때 빠르게 감지하고자 할 경우에 사용될 수 있다.

(리눅스에서 파일 시스템 이벤트 통보 기능을 제공해주는 것 중에는 리눅스 커널 서브시스템 중 하나인 inotify가 있다.)

파일 시스템 이벤트 API는 OS X v10.5부터 사용 가능해진 기술로, 메커니즘은 크게 세 부분으로 구성된다.

- 특별한 장치를 통해 사용자 공간으로 raw event 통지를 전달하는 커널 코드
- 이러한 이벤트 스트림을 필터링하고 통지를 내보내는 데몬
- 시간에 따른 모든 변화 기록을 저장하는 영구 데이터베이스(persistent database)

파일 시스템 이벤트 API는 파일시스템 변화에 대한 세밀한 통지를 받고자 한다거나 특정 파일의 변화를 알아내기 위한 용도보다는 대규모 파일 트리에서의 변화를 수동적으로 모니터링하는 데 적합하게 디자인되었는데, 이에 대한 가장 확실한 예는 백업 소프트웨어다.

보다 기술적인 내용은 아래 애플 개발자 문서를 참조해보길 바라며,
추가로 Jekyll에서 사용되는 FSEvents Ruby Gem과 내부적으로 사용되는 C언어로 작성된 툴인ㄴ fsevent_watch로 살펴보기 바란다.

- [File System Events Programming Guide](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/FSEvents_ProgGuide/Introduction/Introduction.html)
- [rb-fsevent](https://github.com/thibaudgg/rb-fsevent)
- [fsevent_watch](https://github.com/proger/fsevent_watch)
