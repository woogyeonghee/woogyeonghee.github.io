---
layout: post
title: device control (0)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).


# 디바이스 드라이버 읽기/쓰기 함수 정리

## read()

|분류|내용|
|:--------:|:--------|
|기능|디바이스 파일에 ioctl()함수를 수행하였을 때 호출되어 ioctl를 처리한다|
|형태|#include<linux/fcntl.h><br/><br/>int ioctl(struct inode *inode, struct file *filp, unsigned int cmd, unsigned long arg);|
|매개변수|1. inode : 열려진 디바이스 파일에 대한 정보가 있는 구조체 주소<br/>2. filp : 디바이스 드라이버의 처리와 관련된 정보가 있는 구조체 주소<br/>3. cmd : 응용 프로그램에서 전달도니 명령 <br/>4. arg : cmd 명령을 처리하기 위한 부가적 데이터|
|반환값|성공 : 0이 이상 <br/> 실패 : 반환값 < 0 <br/> 반환값은 저수준 파일 입출력 함수인 ioctl()의 반환값을 기준으로 해야 한다|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||
