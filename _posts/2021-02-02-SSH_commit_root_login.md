---
layout: post
title: "Linux - SSH 관련 보안설정- 1"
description: "Linux - SSH 관련 보안설정 - 1"
categories: [linux]
tags: [jekyll, linux, ssh, 보안설정, PermitRootLogin]
redirect_from:
  - /2021/02/02/
---

> This is code blocks and highlighting test page for [Simple Texture][Simple Texture] theme.

* Kramdown table of contents
{:toc .toc}

# ◎Root 접속 제한

많은 사이트에서 보안상의 이유로 SSH 접속을 할때 root로 바로 접근되는것을 막아 놓습니다.

ssh 사용시 가장 기본이 되는 보안설정으로 어떻게 하는지 한번 알아보겠습니다.

## user 생성

말 그대로 root 직접 접근을 막게 될 경우 접속 및 실제 사용자가 사용할 user를 생성해 줍니다.
여기서는 `bbang01` 로 생성하겠습니다.


~~~~~~~~~~~~~~~~~~~~~~ ruby
# useradd -m bbang01
~~~~~~~~~~~~~~~~~~~~~~

`useradd` 명령어를 사용할때는 아래 처럼 간단하게 옵션을 선택해서 적용할수있습니다.

> -m : user생성시 home 디렉터리도 함께 생성 ( ex: /home/bbang01 )
>
> -g : 1차 그룹 지정  /  -G : 2차 그룹 지정
>
> -d : 임의로 home 디렉터리를 지정
>
> -s : 사용자 생성시 사용할 셀을 지정
>
> -u : UID 값을 지정하여 생성

그 뒤에는 생성한 user인 *`bbang01`*의 패스워드를 지정해 줍니다.

~~~~~~~~~~~~~~~~~~~~~~ ruby
# passwd bbang01
Changing password for user bbang01.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
~~~~~~~~~~~~~~~~~~~~~~

생성된 user는 `/etc/passwd` 안에 저장됩니다.

~~~~~~~~~~~~~~~~~~~~~~ ruby
# tail -n 4 /etc/passwd 
tcpdump:x:72:72::/:/sbin/nologin
git_shbang:x:1000:1000:git_shbang:/home/git_shbang:/bin/bash
sh_bbang:x:1001:1001::/home/sh_bbang:/bin/bash
bbang01:x:1002:1002::/home/bbang01:/bin/bash
~~~~~~~~~~~~~~~~~~~~~~

> 사용자이름:암호:사용자ID:그룹ID:추가정보:홈디렉토리:쉘
>
> ( 참고 - 그룹파일은 /etc/group, 비밀번호파일은 /etc/shadow )



## root 직접 접근 제한

ssh의 설정은 `/etd/ssh/` 아래에 있는 파일들의 설정으로 이루어 집니다.

그중에서도 `sshd_config` 을 통해서 설정을 진행해 보도록 하겠습니다.

~~~ ruby
# vi /etc/ssh/sshd_config
PermitRootLogin no // 보통 주석 처리 및 yes로 되어있음
~~~

PermitRootLogin 설정이 yes에서 no로 바뀔 경우 문제가 생겨서 root로만 접속해야할 경우 Console을 통해서만 가능하니 주의해야 합니다.

~~~ ruby
# systemctl restart sshd

or

# systemctl stop sshd; systemctl start sshd
~~~

재시작 한 뒤 접속이 불가능 한 것을 확인합니다.

~~~ ruby
login as: root
root@192.168.197.128's password:
Access denied
root@192.168.197.128's password:
Access denied
root@192.168.197.128's password:
~~~

~~~ ruby
login as: bbang01
bbang01@192.168.197.128's password:
Last login: Sun Dec 13 00:11:33 2020 from 192.168.197.1
[bbang01@bbang ~]$
~~~

# ◎SSH Port 변경

ssh의 포트로 알려진 22번은 모든 사람들이 기본으로 알고있는 *default port* 이기 때문에 변경할 필요가 있습니다.


## config 설정 변경

sshd_config 에서 `Port`부분을 변경해줍니다.

~~~~~~~~~~~~~~~~~~~~~~ ruby
# vi /etc/ssh/sshd_config

#Port 22 //17번 라인 쯤
Port 10001 // 라인 추가 또는 22를 원하는 포트로 변경
~~~~~~~~~~~~~~~~~~~~~~

ssh_config 에서도 'Port'부분을 변경해줍니다.

sshd 재시작은 필수!

~~~ ruby
# systemctl restart sshd

or

# systemctl stop sshd; systemctl start sshd
~~~


[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture
<!--This is [a link](https://yizeng.me) to my homepage.-->
<!--A [link](https://yizeng.me/blog "Yi Zeng's Blog") can also have a title.-->

<!--***전체 굵고 기울여서***.-->

<!--**굴게 _여기만기울여서_ 쓰기**.-->

<!--*전체 _기울임_ 굵지않게*.-->

<!--**굵게 __기울임 없이 언더바 표현은 두번__ 굵게**-->

<!--footnote[^1] / 맨 밑에 [^1]: This is a footnote 로 내용삽입-->

<!--<kbd>keyboard text</kbd>-->

<!-- `코드`-->

<!--<ins>밑줄</ins>-->

<!--_기울임_-->

<!--<strike>취소선</strike>-->

