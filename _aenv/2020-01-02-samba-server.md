---
layout: post
title: environment setting
tags: 
- text
---



# SAMBA-SERVER 리눅스에서 윈도우와 공유할 폴더 만들기 
## 1. 리눅스에 삼바 설치
~~~
sudo apt-get -y install samba
~~~
<br/><br/>

## 2. 공유할 폴더 생성
~~~
mkdir /sharedfolder
chmod777 /sharedfolder
~~~
- 여기서 폴더이름은 원하는데로 
<br/><br/>

## 3. 에디터로 삼바conf 수정
~~~
sudo vim /etc/samba/smb.conf
~~~
- 여기서 에디터는 nano를 가용ㅎ
<br/><br/>

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
<br/><br/>

## 5. samba restart
~~~
sudo service smbd restart
~~~

## 6. 윈도우에서 파일 탐색기를 열고 연동할 ip를 입력
~~~
\\ip
~~~