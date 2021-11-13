---
layout: post
title: ssh-keyggen
tags: 
- text
---
<br/><br/><br/><br/><br/><br/>

# ssh-keyggen 리눅스와 윈도우 환경에서 생성하는 법

- ssh(Secure Shell) : 네트워크 상에서 다른 호스트컴퓨터로 접근하기 위해 사용되는 프로토콜이다

- github repository를 remote할 때 ssh-keygen이 사용된다.
<br/><br/><br/>

# ssh-keygen 생성하는 법 (리눅스)


## 1. 리눅스 홈디랙토리로 이동한다
~~~
$ cd
~~~

## 2.  ssh-keygen 생성 
~~~
$ ssh-keygen -t rsa
~~~

- t :어떤 암호화 방식을 사용할것인지 결정

- rsa : private key 추출하는 암호화 방식

## 3. 엔터 두번으로 경로와 pass phrase 빈값으로 지정
~~~
Enter
Enter
~~~

## 4. i_rsa.pub을 확인하기
~~~
$ cat ~/.ssh/i_rsa.pub
~~~
- cat : 파일의 내용을 콘솔창에 출력
<br/>
<br/>
<br/>

# ssh-keygen 생성하는 법 (윈도우)

## 1. 마우스 오른쪽 클릭 후 git bash 생성 
~~~
cd
~~~

## 2. ssh-keygen 생성
~~~
ssh-keygen
~~~

## 3. 생성한 후 cli 화면에 표시된 경로를 확인


## 4. 경로 복사후 해당 폴더로 이동


## 5. id_rsa 클릭


## 6. ssh-keygen 복사


