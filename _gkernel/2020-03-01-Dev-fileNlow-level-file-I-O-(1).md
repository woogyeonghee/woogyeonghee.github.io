---
layout: post
title: Dev file & low level file I/O (2)
tags: 
- text
---


### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).

# 기본적인 저수준 파일 입출력 함수 예제


## dev/port device file

 - Iseek : 접근할 I/O 주소를 지정
 - read : 지정된 I/O에서 데이터를 읽어온다
 - write : 지정된 I/O에 데이터를 쓴다



![hello](https://user-images.githubusercontent.com/88933098/147305837-de773e89-2d19-4a74-aa69-eb4c41e97fc4.png)


### 예제 소스
~~~
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main( int argc, char **argv )
{
    int fd;
    int lp;
    
    unsigned char  buff[128];
    
    fd = open( "/dev/port", O_RDWR );   
    if( fd < 0 ) 
    {
        perror( "/dev/port open error" );
        exit(1);
    }    
    
    for( lp = 0; lp < 10; lp++ )
    {
        lseek( fd, 0x378, SEEK_SET );
        buff[0] = 0xFF;
        write( fd, buff, 1 );
        sleep( 1 );
        lseek( fd, 0x378, SEEK_SET );
        buff[0] = 0x00;
        write( fd, buff, 1 );
        sleep( 1 );
    }
    close( fd );
    
    return 0;
}

~~~

### 실행방법
~~~ 
root# gcc -o test main.c
root# ./test
~~~