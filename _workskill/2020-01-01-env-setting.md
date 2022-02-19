---
layout: post
title: environment setting network
tags: 
- text
---

<br/><br/><br/><br/><br/><br/>

# SAMBA-SERVER 리눅스에서 윈도우와 공유할 폴더 만들기 
## 1. 리눅스에 삼바 설치
~~~
sudo apt-get -y install samba
~~~

## 2. 공유할 폴더 생성
~~~
mkdir /sharedfolder
chmod777 /sharedfolder
~~~
- 여기서 폴더이름은 원하는데로 

## 3. 에디터로 삼바conf 수정
~~~
sudo vim /etc/samba/smb.conf
~~~
- 여기서 에디터는 nano를 사용해도 가능

## 4. 맨 마지막 줄엔 아래의 내용을 추가
~~~
[Shared Folder]
  comment = First Shared Folder
  path = pwd경로/sharedfolder
  public = yes
  writable = yes
~~~
- 여기서 pwd 경로는 sharedfoder 의 경로를 입력해 주면 됨
- 띄어쓰기 주의 할 것 (위에있는 내용과 비교하여)

## 5. samba restart
~~~
sudo service smbd restart
~~~

## 6. 윈도우에서 파일 탐색기를 열고 연동할 ip를 입력
~~~
\\ip
~~~

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
