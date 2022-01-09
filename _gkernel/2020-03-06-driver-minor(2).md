---
layout: post
title: driver minor & major (2)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).

# 부 번호에 의한 파일 처리 예제

# 부 번호에 의한 파일 처리 예제 2

~~~
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/ioctl.h>
#include <fcntl.h>
#include <unistd.h>

#define READ_DEVICE_FILENAME   "/dev/minor_read"
#define WRITE_DEVICE_FILENAME  "/dev/minor_write"

int main()
{
    int  read_dev;
    int  write_dev;
    
    char buff[128];
    int loop;

    read_dev  = open( READ_DEVICE_FILENAME,  O_RDWR|O_NDELAY );
    if( read_dev < 0 ) 
    {  
        printf( READ_DEVICE_FILENAME "open error\n" );
        exit(1);
    }
    
    write_dev = open( WRITE_DEVICE_FILENAME, O_RDWR|O_NDELAY );
    if( write_dev < 0 ) 
    {  
        printf( WRITE_DEVICE_FILENAME "open error\n" );
        close( read_dev );
        exit(1);
    }
    
    printf( "wait... input\n" );
    while(1)
    {
        if( read(read_dev,buff,1 ) == 1 )
        {
            printf( "read data [%02X]\n", buff[0] & 0xFF );
            if( !(buff[0] & 0x10) ) break;
        }
    } 
    printf( "input ok...\n");
    
    printf( "led flashing...\n");
    for( loop=0; loop<5; loop++ )
    {
        buff[0] = 0xFF; 
        write(write_dev,buff,1 );
        sleep(1);
        buff[0] = 0x00; 
        write(write_dev,buff,1 );
        sleep(1);
    }
        
    close(read_dev);
    close(write_dev);

    return 0;
}


~~~
