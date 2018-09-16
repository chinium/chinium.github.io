---
layout: post
title: Install PIP on CentOS 6
date: 2018-08-30 12:00
tags: [pip, centos6]
categories:
  - python
---
## PIP
- A tool for installing and managing Python packages  
- https://pypi.python.org/pypi/pip  
   

### 설치
{% highlight bash %}
$ yum install python-pip
{% endhighlight %}


### 아래와 같은 SSL 관련 에러 메세지가 나오는 경우
{% highlight bash %}
Could not fetch URL https://pypi.python.org/simple/Django/: There was a problem confirming the ssl certificate:
{% endhighlight %}


### pip 1.5.6 소스 설치 후 pip upgrade
{% highlight bash %}
wget https://pypi.python.org/packages/source/p/pip/pip-1.5.6.tar.gz -O - | \
tar xvfpz - && cd pip-1.5.6 && python setup.py install
{% endhighlight %}
