---
layout: post
title: driver control (2)
tags: 
- text
---

### 도서출제
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).



#  loctl()함수를 이용한 입출력 구현 header

~~~
#ifndef _IOCTLTEST_H_  
#define _IOCTLTEST_H_  
  
#define IOCTLTEST_MAGIC    't'  
  
typedef struct 
{ 
	unsigned long size;  
	unsigned char buff[128];  
} __attribute__ ((packed)) ioctl_test_info; 
  
#define IOCTLTEST_LEDOFF           _IO(  IOCTLTEST_MAGIC, 0 )  
#define IOCTLTEST_LEDON            _IO(  IOCTLTEST_MAGIC, 1 )  
#define IOCTLTEST_GETSTATE         _IO(  IOCTLTEST_MAGIC, 2 )   
  
#define IOCTLTEST_READ             _IOR( IOCTLTEST_MAGIC, 3 , ioctl_test_info )  
#define IOCTLTEST_WRITE            _IOW( IOCTLTEST_MAGIC, 4 , ioctl_test_info )  
#define IOCTLTEST_WRITE_READ       _IOWR( IOCTLTEST_MAGIC, 5 , ioctl_test_info )  
  
#define IOCTLTEST_MAXNR                                   6  
    
#endif // IOCTLTEST_H_  

~~~


#  loctl()함수를 이용한 입출력 구현 예제 2

~~~
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/ioctl.h>
#include <fcntl.h>
#include <unistd.h>

#include "ioctl_test.h"

#define DEVICE_FILENAME  "/dev/ioctldev"

int main()
{
    ioctl_test_info  info;
    int              dev;
    int              state;
    int              cnt;
    
    dev = open( DEVICE_FILENAME, O_RDWR|O_NDELAY );
    if( dev >= 0 )
    {
                 
        printf( "wait... input\n" );
        ioctl(dev, IOCTLTEST_LEDON ); 
        while(1)
        {
            state = ioctl(dev, IOCTLTEST_GETSTATE );
            if( !(state & 0x10) ) break;
        } 
        
        sleep(1);
        ioctl(dev, IOCTLTEST_LEDOFF ); 
        
        printf( "wait... input\n" );
        while(1)
        {
            info.size = 0;
            ioctl(dev, IOCTLTEST_READ, &info );
            if( info.size > 0 ) 
            {
                if( !(info.buff[0] & 0x10) ) break;
            }
            
        }    
        info.size = 1;
        info.buff[0] = 0xFF;
        for( cnt=0; cnt<10; cnt++ )
        {
            ioctl(dev, IOCTLTEST_WRITE, &info );
            
            info.buff[0] = ~info.buff[0];
            usleep( 500000 );
        }
        
        printf( "wait... input\n" );
        cnt   = 0;
        state = 0xFF;
        
        while(1)
        {
            info.size    = 1;
            info.buff[0] = state;
            ioctl(dev, IOCTLTEST_WRITE_READ, &info );
            
            if( info.size > 0 ) 
            {
                if( !(info.buff[0] & 0x10) ) break;
            }
            
            cnt++;
            if( cnt >= 2 )
            { 
                cnt = 0; 
                state = ~state;
            }     
            usleep( 100000 );
        }
        ioctl(dev, IOCTLTEST_LEDOFF ); 

        close(dev);
    }

    return 0;
}


~~~

## Makefile

~~~
obj-m	:= ioctl_dev.o

KDIR	:= /lib/modules/$(shell uname -r)/build
PWD	:= $(shell pwd)

default:
	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
clean:
	rm -rf *.ko
	rm -rf *.mod.*
	rm -rf .*.cmd
	rm -rf *.o
~~~