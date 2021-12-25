---
layout: post
title: Dev file & low level file I/O (0)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).

# 저수준 파일의 입출력 흐름


![hello](https://user-images.githubusercontent.com/88933098/147304725-ebbf4139-546c-477b-9000-b940694cbb39.png)




## 1.명령어

## 1.1 mknod : 특수 파일 생성

|분류|내용|
|------|------------------------------|
|기능|특수 파일을 만든다|
|형태|# mknod [options] 디바이스파일명 {bcu} 주번호 부번호</br> 옵션 : [-m mode] [--mode=mode] [--help] [--version]|
|옵션| > -m, --mode mode : 생성된 파일의 모드를 지정하는 옵션이다. mode로 사용할 것은 chmod에서 사용하는 기호나 숫자 형식이다</br> > --help : 도움말을 보여주고 마친다</br> > --version : 버전 정보를 보여준다 |
|설명|1. mknod 명령은 FIFO, 문자 디바이스 파일, 블록 디바이스 파일 등을 만드는데 사용된다. </br>2. 파일 모드 (초기값) :0666</br>3. 파일의 특수 형태를 알리는 값 </br>&nbsp;&nbsp; > p : FIFO </br>&nbsp;&nbsp; > b : 블록 디바리스 파일 </br>&nbsp;&nbsp; > c 또는 u : 문자 디바이스 파일</br> * 블록이나 문자 특수 파일을 만드는 경우에는 그 디바이스 파일의 주 번호와 부 번호를 지정해야한다|


 </br> 

## 2.함수

## 2.2 open() & close()
## 2.1.1 open() : 파일이나 장치를 연다

|분류|내용|
|------|------------------------------|
|기능|디바이스 파일을 연다|
|형태|#include<sys/types.h></br>#include<sys/stat.h></br>#include<fcntl.h></br></br>int open(const char *pathname, int flags);|
|설명|1. pathname : 지정된 문자열로 표현되는 디바이스 파일 </br> &nbsp;(일반적으로 pathname에 지정되는 위치는 '/dev/'디렉토리에 있는 디바이스 파일이다)</br>2. flags : 지정된 속성 |
|매개변수|1. pathname : 디바이스 파일을 지정하는 문자열의의 주소를 지정한다 </br>2. flags : 디바이스 파일에 접근하기 위한 속성을 지정한다</br> &nbsp;&nbsp;>> O_RDONLY : 읽기 전용으로 연다</br> &nbsp;&nbsp;>> O_WRONLY : 쓰기 전용으로 연다</br> &nbsp;&nbsp;>> O_RDWR : 읽기 쓰기가 가능하게 연다</br> &nbsp;&nbsp;>> O_NOCTY : 디바이스 파일이 터미널 디바이스일 때 시리얼 디바이스로 속성을 바꾼다 </br> &nbsp;&nbsp;>> O_NONBLOCK : 파일은 비블록 모드로 열린다 </br> &nbsp;&nbsp;>> O_NDELAY : 파일은 비블록 모드로 모드로 열린다 </br> &nbsp;&nbsp;>> O_SYNC : 파일은 입출력 동기화를 위해 열린다 디바이스에 쓴 내용이 하드웨어에 기록될 때까지 호출 프로세스는 블록 상태가 된다 |
|반환값|파일 디스크립터를 반환: 성공 </br>-1 : 실패</br> - error 값 </br>&nbsp;&nbsp; > ENXIO : 파일이 디바이스 파일이고 일치하는 디바잇그가 없다</br> &nbsp;&nbsp; > ENODEV : 디바이스 파일에 관련된 디바이스 드라이버가 없거나 하드웨어가 없다 </br> &nbsp;&nbsp; > ENOMEM :  커널메모리가 부족하다 |

 </br> 


## 2.1.2 close() : 열린파일을 닫는다

|분류|내용|
|------|------------------------------|
|기능|디바이스 파일을 닫는다|
|형태|#include<unistd.h></br></br>int close(int fd);|
|설명|1. close : opend() -> fd을 닫음.|
|매개변수| fd : 함수 실행결과로 반환된 파일 디스크립터.|
|반환값| 0 : 닫기 성공</br>-1 : 닫기 실패|
   
</br> 
  
## 2.1.3 예제 - open & close

  ~~~
  int fd = -1;
  fd = open( "/dev/mem", O_RDWR | O_NONBLOCK);
  if( fd < 0 )
  {
      //에러처리
  }
  close(fd);
  ~~~

</br>

## 2.2 : read() & write()
## 2.2.1 read() : 파일에서 데이터를 읽어온다

|분류|내용|
|------|------------------------------|
|기능|디바이스 파일에서 데이터를 읽는다|
|형태|#include <unistd.h> </br></br>ssize_t read(int fd, void *buf, size_t count);|
|설명|1. read() : fd에 데이터를 count만큼 read 한 후 buf -> 주소에 넣음( count < SSIZE_MAX). </br> 2.  O_NONBLOCK, O_NDELAY가 지정 X : count 값의 크키에 해당하는 데이터가 읽혀질 때까지 블록됨</br>&nbsp;(옵션을 지정하더라도 블록될 수 있다 원칙적으로 이런 경우는 잘못된 디바이스 드라이버다). </br> * 함수의 실행 결과로 파일 포인터의 위치는 읽은 수만큼 이동한다|
|매개변수|1. fd : 홤수 실행 경과로 반환된 파일 디스크립터</br>2. buf : 읽은 데이터가 저장될 공간의 주소.</br> &nbsp;(주소가 가리키는 공간의 크기 > count값) </br>3. count : 디바이스 파일에서 읽어올 데이터의 크기</br>&nbsp;(count< SSIZE_MAX, if count==0 즉시 실행 종료)|
|반환값|읽은 바이트수 : 성공 </br>-1 : 실패 </br>- error 값</br> &nbsp;&nbsp;>> EINTR : 어떤 데이터를 읽기도 전에 함수가 신호에 의해 인터럽트 되었다</br> &nbsp;&nbsp;>> EAGIAN : O_NONBLOCK으로 열렸지만 read 호출 시에 즉시 읽을 수 있는 데이터가 없다</br> &nbsp;&nbsp;>> EIO : 디바이스 파일에서 데이터를 읽는 동안 I/O 에러가 발생했다</br> &nbsp;&nbsp;>> EBADF : fd가 유효한 파일 디스크립터가 아니거나 읽기 위해 열리지 않았다 </br> &nbsp;&nbsp;>> EINVAL : fd가 읽기에 적당하지 않는 객체와 연결되었다 </br> &nbsp;&nbsp;>> EFAULT : buf가 접근할 수 없는 주소 공간을 가리키고 있다|

## 2.2.2 예제 - read
  ~~~
  ret_num = read(fd, buff, 10);
  if(ret_num < 0)
  {
      //에러처리
  }
  if(ret_num != 10)
  {
      //요구된것과 다를 때 처리
  }
  ~~~ 
  
 </br> 
  
## 2.2.3 write() : 파일에 데이터를 쓴다
  
분류|내용
|------|------------------------------|
|기능|디바이스 파일에 데이터를 쓴다|
|형태|#include <unistd.h> </br></br>ssize_t write(int fd, void *buf, size_t count);|
|설명|1. write() : buf가 가르키는 데이터를 count만큼 fd에 해당하는 디바이스 파일에 넣음 ( count < SSIZE_MAX ). </br> 2.  O_NONBLOCK, O_NDELAY가 지정 X : count 값의 크키에 해당하는 데이터가 읽혀질 때까지 블록됨</br>&nbsp;(옵션을 지정하더라도 블록될 수 있다 원칙적으로 이런 경우는 잘못된 디바이스 드라이버다). </br> * 함수의 실행 결과로 파일 포인터의 위치는 데이터를 넣은 수만큼 이동한다|
|매개변수|1. fd : open() 함수 실행 결과로 반환된 파일 디스크립터</br>2. buf : 쓸 데이터가 저장될 공간의 주소.</br> &nbsp;(주소가 가리키는 공간의 크기 > count값) </br>3. count : 디바이스 파일에 쓸 데이터의 크기</br>&nbsp;(count< SSIZE_MAX, if count==0 즉시 실행 종료)|
|반환값|쓴 만큼의 바이트수 : 성공</br>-1 : 실패 </br>- error 값</br> &nbsp;&nbsp;>> EBADF : fd가 유효한 파일 디스크립터가 아니거나 쓸 쑤 있게 열려있지 않다</br> &nbsp;&nbsp;>> EINVAL : fd가 쓰기에 적당하지 않은 객체와 연결되었다 </br> &nbsp;&nbsp;>> EFAULT : buf는 접근할 수 없는 주소 공간을 가리키고 있다.</br> &nbsp;&nbsp;>> ENGAIN : O_NONBLOCK으로 열렸지만 write 호출 시에 즉시 처리할 수 있는 상황이 아니다.</br> &nbsp;&nbsp;>> EINTR : 어떤 데이터를 모두 쓰기 전에 신호에 의해 함수가 인터럽트되었다 </br> &nbsp;&nbsp;>> ENOSPC : fd로 참조되는 파일을 포함하는 디바이스에 데이터를 위한 공간이 없다 </br> &nbsp;&nbsp;>> EIO : 디바이스 파일에서 데이터를 쓰는 동안 I/O 에러가 발생했다|

 </br> 

## 2.2.4 예제 - write
  ~~~
  ret_num = write(fd, buff, 10);
  ~~~


 </br> 

## 2.3 lseek()

|분류|내용|
|------|------------------------------|
|기능|읽기 또는 쓰기의 위치를 변경한다|
|형태|#include<sys/types.h> </br>#include<unistd.h> </br></br> off_t lseek(int fd, off_t offset, int whence|
|설명|1. 파일디스크립터 fd에 해당하는 디바이스 파일에서 파일 포인터의 위치를 변경한다</br>&nbsp; (whence가 지정하는 옵션값에 따라 해석되는 offset 값의 위치로 파일 포인터의 위치를 변경한다)</br>2. 변경된 파일 포인터의 위치는 디바이스 파일에 연동된 디바이스 드라이버가 어떻게 해석되냐에 따라 여러가지로 해석될 수 있다</br>&nbsp; (ex. 메모리를 관리하는 디바이스라면 메모리의 주소로 이용될 수 있다|
|매개변수|1. fd : opne() 함수 실행 결과로 반환된 파일 디스크립터 </br>2. offset : 이동할 파일 포인터의 위치를 바이트 단위로 지정한다. 이 값은 음수값이다. 이 값은 whence에 따라 실제 이동 위치로 해석된다 </br>3. whence : offset을 해석하기 위한 조건을 지정한다</br> &nbsp;&nbsp;>> SEEK_SET : 이동 위치는 offset이 가리키는 값을 그대로 적용한다</br> &nbsp;&nbsp;>> SEEK_CUR : 이동 위치는 현재위치에 offset값을 더한 위치가 적용된다</br> &nbsp;&nbsp;>> SEEK_END : 이동 위치는 맞지막 위치에서 offset을 뺸 위치로 정한다. 디바이스 파일은 대부분 끝에 대한 정의가 애매하기 때문에 이 값을 잘 사용하지 않는다|
|반환값|파일시작에서 바이트로 측정된 결과 변위 위치 : 성공 </br> (off_t)-1 : 실패 </br>-error</br> &nbsp;&nbsp;>> EINVAL : whence에 지정한 값이 적당하지 않다|

## 2.3.1 lseek : 접근할 I/O 주소를 읽는다
  
## 2.3.2 예제 - lseek
  ~~~
  off_t ret_pos;
  ...
  ...
  ret_pos = Iseek(fd, 1234, SEEK_CUR)다
  ~~~


 </br> 

## 2.4 ioctl()

|분류|내용|
|------|------------------------------|
|기능|디바이스 파일을 제어한다|
|형태|#include<sys/ioctl.h></br></br>int ioctl(int fd, int request,...);|
|설명|1. read()와 write()로 처리하기 힘든 입출력에 사용된다</br>2. 실질적인 각 값의 의미는 각 디바이스 파일마다 다르게 해석된다</br>3.ioctl() 함수의 매개변수의 수는 가변적으로 문법상 표현되기는 하나 최대 3개 까지 허용된다.</br> &nbsp; (세번째인자는 전통적으로 char *argp로 표현한다) |
|매개변수|1. fd : open() 함수의 실행결과로 반환된 파일 디스크립터 </br>2. request: 디바이스 파일에 연동된 디바이스 드라이버에세 취해야 할 명령을 정의한다. 이 값은 매크로에 의해서 출력용 명령인지 입력용 명령인지 argp기 지정하는 값이 기억 공간의 주소일 경우 전달활 인자에 대하여 바이트 단위의 크기를 나타낸다 </br>3. ... : 세번째 매개변수는 argp로 지칭되기도 하는데 request에 따른다. request 명령을 처리하는 보조적인 정보값이다|
|반환값|0 : 성공 </br> -1 : 실패</br>-error</br> &nbsp;&nbsp;>> EFAULT : argp는 접근할 수 없는 메모리 영역을 가리킨다</br> &nbsp;&nbsp;>> ENOTTY : fd는 문자 디바이스 파일과 연관되어 있지 않다 </br> &nbsp;&nbsp;>> EINVAL : 디바이스 파일에 연동된 디바이스 드라이버가 request 또는 argp를 처리할 수 없다 |

## 2.4.1 ioctl() : read(),write()로 다루지 않는 특수한 제어를 한다

  
## 2.5 ioctl()
|분류|내용|
|------|------------------------------|
|기능|디바이스 파일에서 하드웨어에 적용되지 않은 상태를 즉시 적용시켜 동기화 한다|
|형태|#include<unist.h></br></br>int fsync(int fd);|
|설명|1. fsync 함수는 디바이스 파일에 쓴 데이터가 디바이스 드라이버의 실제 디바이스에서 모두 처리하게 한다. 처리되는 동안에 이 함수를 호출한 프로세스는 블록된다|
|매개변수|1. fd : open() 함수 실행 결돠로 반환된 파일 디스크립터|
|반환값|0 : 성공 </br>-1 : 실패</br>-error</br> &nbsp;&nbsp;>> EROSFS,EINVAL : 디바이스 파일이 이 함수에 대한 처리를 지원하지 않는다</br> &nbsp;&nbsp;>> EIO : 동기화 하는동안 에러가 발생했다|


 </br> 
 
## 2.5.1 fsync() : 파일에 쓴 데이터와 실제 하드웨어의 동기를 맞춘다

## 2.5.2 예제 - fsync

  ~~~
  int ret;
  ...
  ...
  ret = fsync(fd);
  ~~~


 </br> 







