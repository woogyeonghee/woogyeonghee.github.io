---
layout: post
title: driver read & write (0)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).


# 디바이스 드라이버 읽기/쓰기 함수 정리

## read()

|분류|내용|
|:--------:|:--------|
|기능|응용프로그램에서 디바이스 파일에 read를 수행했을 때 호출되는 함수로 읽기를 처리한다|
|형태|#include<linux/fs.h><br/><br/>ssize_t read(struct file *filp, const char *buf, size_t count, loff_t *f_pos);|
|매개변수|1. filp : 열린 파일에 대한 연산을 위한 구조체 주소<br/>2. buf : 응용프로그램에서 전달된 데이터의 선두 주소(사용자공간)<br/> 3. count : 읽고자 요청한 데이터 개수<br/> 4. f_pos : 파일의 위치 변수(seek 값)|
|반환값|성공 : 처리된 데이터 개수 <br/> 실패 : 반환값 < 0 <br/> 반환값은 저수준 파일 입출력 함수인 read의 반환값을 기준으로 해야한다|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## write()

|분류|내용|
|:--------:|:--------|
|기능|응용 프로그램에서 디바이스 파일에 write를 수행했을 떄 호출되는 함수로, 쓰기를 처리한다|
|형태|#include <linux/fs.h><br/><br/>ssize_t write(struct file *filp, const char *buf, sizr_t count, loff_t *f_pos);|
|매개변수|1. filp : 열린 파일에 대한 연산을 위한 구조체 주소<br/>2. buf : 응용프로그램에서 전달된 데이터의 선두 주소(사용자공간)<br/> 3. count : 읽고자 요청한 데이터 개수<br/> 4. f_pos : 파일의 위치 변수(seek 값)|
|반환값|성공 : 처리된 데이터 개수 <br/> 실패 : 반환값 < 0 <br/> 반환값은 저수준 파일 입출력 함수인 write의 반환값을 기준으로 해야한다|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## inb(), inw(), inl()

|분류|내용|
|:--------:|:--------|
|기능|i386과 같은 I/O 영역과 메모리 영역이 구분되어 있는 프로세스의 I/O 영역에서 데이터를 읽어온다. <br/>1. inb() : I/O에서 비트 8비트 데이터(1바이트)를 읽는다<br/>2. inw() : I/O에서 비트 16비트 데이터(2바이트)를 읽는다<br/>3. inl() : I/O에서 비트 32비트 데이터(4바이트)를 읽는다|
|형태|#include <asm.io.h><br/><br/>unsigned char inb(unsigned short port)<br/>unsigned short inw(unsigned short port)<br/>unsigned long inl(unsigned short port)|
|매개변수|1. port : I/O 주소|
|반환값| I/O 영역에서 읽어들인 데이터값|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

##  insb(), insw(), insl()

|분류|내용|
|:--------:|:--------|
|기능|1. insb() : I/O에서 비트 8비트 스트림 데이터(1바이트)를 읽는다<br/>2. insw() : I/O에서 비트 16비트 스트림 데이터(2바이트)를 읽는다<br/>3. insl() : I/O에서 비트 32비트 스트림 데이터(4바이트)를 읽는다|
|형태|#include <asm.io.h><br/><br/>void insb(unsigned short port, void *addr, unsigined long count)<br/>void insw(unsigned short port, void *addr, unsigined long count)<br/>void insl(unsigned short port, void *addr, unsigined long count)|
|설명| i286과 같은 I/O 영역과 메모리 영역이 구분되어 있는 프로세스의 I/O 영역어서 port가 지정된 주소에 데이터를 연속적으로 count 만큼 addr이 가리키는 메모리 공간에 써넣는다|
|매개변수|1. port : I/O주소 <br/> 2. addr : 메모리 버퍼 선두 주소<br/>count : 읽어들일 데이터 개수|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

##  outb(), outw(), outl()

|분류|내용|
|:--------:|:--------|
|기능|i386과 같은 I/O 영역과 메모리 영역이 구분괴어 있는 프로세스의 I/O 영역에서 데이터를 써넣는다<br/>1. outb() : I/O에서 비트 8비트 데이터(1바이트)를 쓴다<br/>2. outw() : I/O에서 비트 16비트 데이터(2바이트)를 쓴다<br/>3. outl() : I/O에서 비트 32비트 데이터(4바이트)를 쓴다|
|형태|#include <asm.io.h><br/><br/>void outb(unsigned char data, unsigned short port)<br/>void outw(unsigned char data, unsigned short port)<br/>void outl(unsigned char data, unsigned short port)|
|매개변수|1. port : I/O주소 <br/>2. data : 데이터|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

##  outsb(), outsw(), outsl()

|분류|내용|
|:--------:|:--------|
|기능|1. outsb() : I/O에서 비트 8비트 스트림 데이터(1바이트)를 쓴다<br/>2. outsw() : I/O에서 비트 16비트 스트림 데이터(2바이트)를 쓴다<br/>3. outsl() : I/O에서 비트 32비트 스트림 데이터(4바이트)를 쓴다|
|형태|#include <asm.io.h><br/><br/>void outsb(unsigned short port, void *addr, unsigned long count)<br/>void outsw(unsigned short port, void *addr, unsigned long count)<br/>void outsl(unsigned short port, void *addr, unsigned long count)|
|매개변수|1. port : I/O주소 <br/> 2. addr : 메모리 버퍼 선두 주소<br/>count : 읽어들일 데이터 개수|
|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## readb(), readw(), readl()

|분류|내용|
|:--------:|:--------|
|기능|메모리 맵드 I/O 영역에서 데이터를 읽어온다<br/>1. readb() : I/O에서 비트 8비트 데이터(1바이트)를 읽는다<br/>2. readw() : I/O에서 비트 16비트 데이터(2바이트)를 읽는다<br/>3. readl() : I/O에서 비트 32비트 데이터(4바이트)를 읽는다|
|형태|#include <asm/io.h><br/><br/>unsigned char readb(unsigned int addr)<br/>unsigned char readw(unsigned int addr)<br/>unsigned short readl(unsigned int addr)|
|매개변수|1. addr : I/O주소|
|반환값| I/O 영역에서 읽어들인 데이터 값|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## writeb(), writew(), writel()

|분류|내용|
|:--------:|:--------|
|기능|메모리 맵드 I/O 영역에서 데이터를 쓴다<br/>1. writeb() : I/O에서 비트 8비트 데이터(1바이트)를 쓴다<br/>2. wirtew() : I/O에서 비트 16비트 데이터(2바이트)를 쓴다<br/>3. writel() : I/O에서 비트 32비트 데이터(4바이트)를 쓴다|
|형태|#include <asm/io.h><br/><br/>void writeb(unsigned char data, unsigned short addr)<br/>void writew(unsigned char data, unsigned short addr)<br/>void writel(unsigned char data, unsigned short addr)|
|매개변수|1. addr : I/O주소<br/>2. data : 데이터|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||


## readsb(), readsw(), readsl()

|분류|내용|
|:--------:|:--------|
|기능|1. readb() : I/O에서 비트 8비트 스트림 데이터(1바이트)를 읽는다<br/>2. readw() : I/O에서 비트 16비트 스트림 데이터(2바이트)를 읽는다<br/>3. readl() : I/O에서 비트 32비트 스트림 데이터(4바이트)를 읽는다|
|형태|#include <asm/io.h><br/><br/>void readsb(unsigned short ioaddr, void *addr, unsigned long count)<br/>void readsw(unsigned short ioaddr, void *addr, unsigned long count)<br/>void readsl(unsigned short ioaddr, void *addr, unsigned long count)|
|매개변수|1. ioaddr : I/O 주소<br/>2. addr : 메모리 버퍼 선두 주소 <br/> 3. count : 읽어들일 데이터 개수|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## writesb(), writesw(), writesl()

|분류|내용|
|:--------:|:--------|
|기능|메모리 맵드 I/O 영역에서 데이터를 쓴다<br/>1. writeb() : I/O에서 비트 8비트 스트림 데이터(1바이트)를 쓴다<br/>2. wirtew() : I/O에서 비트 16비트 스트림 데이터(2바이트)를 쓴다<br/>3. writel() : I/O에서 비트 32비트 스트림 데이터(4바이트)를 쓴다|
|형태|#include <asm/io.h><br/><br/>void writesb(unsigned short ioaddr, void *addr, unsigned long count)<br/>void writesw(unsigned short ioaddr, void *addr, unsigned long count)<br/>void writesl(unsigned short ioaddr, void *addr, unsigned long count)|
|매개변수|1. ioaddr : I/O 주소<br/>2. addr : 메모리 버퍼 선두 주소 <br/> 3. count : 읽어들일 데이터 개수|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||


## verify_area()

|분류|내용|
|:--------:|:--------|
|기능|사용자 메모리 공간의 유효성을 검사한다|
|형태|#include <asm/uaccess.h><br/><br/>int verify_area(int type, const void __user *addr, unsigned long size)|
|설명|사용자 메모리 영역을 기리키는 addr 주소부터 size로 지정되는 크기의 공간이 읽기 또는 쓰기에 유효한 공간인가를 확인한다|
|매개변수|1. type : 읽기 또는 쓰기 모드<br/> &nbsp;&nbsp;>> VERIFY_WRITE : 쓰기 공간에 대한 검사<br/> &nbsp;&nbsp;>> VERIFY_READ : 읽기 공간에 대한 검사<br/> 2. addr : 사용자 메모리 블록 선두 주소<br/> 3. size : 써넣을 바이트 단위의 크기|
|반환값|성공 : 반환값 > 0 <br/> 실패 < 0|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## copy_to_user()

|분류|내용|
|:--------:|:--------|
|기능|커널 메모리 블록 데이터를 사용자 메모리 블록 데이터에 써넣는다|
|형태|#include <asm/uaccess.h><br/><br/>int copy_to_user(void __user *to, const void *from, unsigned long n);|
|설명|from이 가리키는 주소의 커널 메모리 블록 데이터를 to가 가리키는 사용자 메모리 블록 데이터레 바이트 크기 단위인 n 만큼 써넣는다. 이 함수는 써넣은 공간의 유효성 검사를 수행한다|
|매개변수|1. from : 커널 메모리 블록 선두 주소 <br/> 2. to : 사용자 메모리 블록 선두 주소<br/>3.n : 써넣을 바이트 단위의 크기|
|반환값|성공 : 반환값 > 0 <br/> 실패 < 0|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## copy_from_user()

|분류|내용|
|:--------:|:--------|
|기능|사용자 메모리 블록 데이터를 커널 메모리 블록 데이터에 써넣는다|
|형태|#include <asm/uaccess.h><br/><br/>int copy_from_user(void *to, const void __user *from, unsigned long n);|
|설명|from이 가리키는 주소의 사용자 메모리 블록 데이터를 to가 가리키는 커널 메모리 블록 데이터레 바이트 크기 단위인 n 만큼 써넣는다. 이 함수는 써넣은 공간의 유효성 검사를 수행한다|
|매개변수|1. from : 사용자 메모리 블록 선두 주소 <br/> 2. to : 커널 메모리 블록 선두 주소<br/>3.n : 써넣을 바이트 단위의 크기|
|반환값|성공 : 반환값 > 0 <br/> 실패 < 0|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||


## get_user()

|분류|내용|
|:--------:|:--------|
|기능|사용자 공간의 데이터를 읽는다|
|형태| get_user(x,ptr)|
|설명|get_user 매크로 함수는 ptr이 가리키는 사용자 공간의 메모리에서 x 변수 크기 만큼 읽어 온다|
|매개변수|1. x : 커널 변수<br/>2. ptr : 사용자 메모리 블록의 선두 주소|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||

## put_user()
|분류|내용|
|:--------:|:--------|
|기능| 커널 변수값을 사용자 공간에 써넣는다|
|형태| put_user(x,ptr)|
|설명|put_user 매크로 함수는 ptr이 가리키는 사용자 공간의 메모리에 x 변수 크기 만큼 읽어들여 써넣는다|
|매개변수|1. x : 커널 변수<br/>2. ptr : 사용자 메모리 블록의 선두 주소|
|&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;||