---
layout: post
title: Dev file & low level file I/O (2)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).

# ioctl() 함수의 사용 예

## dev/lp0 디바이스 파일과 ioctl() 함수

![hello](https://user-images.githubusercontent.com/88933098/147309050-bd4efdf2-deaa-4446-bc6a-9c9473b7d8ef.png)

## 예제 소스

~~~
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

#include <linux/lp.h>

int main( int argc, char **argv )
{

    int fd;
    int prnstate;   
    int lp;
    
    unsigned char  buff[128];
    
    fd = open( "/dev/lp0", O_RDWR | O_NDELAY );
    if( fd < 0 ) 
    {
        perror( "open error" );
        exit(1);
    }    
  
    while( 1 )
    {  
        ioctl( fd, LPGETSTATUS, &prnstate );
        
        // 13 Pin <--> GND Pin
        if( prnstate & LP_PSELECD ) printf( "ON\n" );
        else                        printf( "OFF\n" ); 
        usleep( 50000 );
    }
    
    close( fd );
    
    return 0;

}
~~~

## 실행 방법
~~~ 
root# gcc -o test main.c
root# ./test
~~~

