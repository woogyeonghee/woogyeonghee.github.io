---
layout: post
title: driver minor & major (0)
tags: 
- text
---

### 도서출처
유영창, 『리눅스 디바이스 드라이버』, 한빛미디어(2010).

# 부 번호에 의한 파일 처리 예제

![hello](https://user-images.githubusercontent.com/88933098/148670479-2236e31c-20dd-46be-a2f6-a65a789a32db.png)

#  번호에 의한 파일 처리 예제 1

~~~
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

#include <linux/fs.h>          
#include <linux/errno.h>       
#include <linux/types.h>       
#include <linux/fcntl.h>       

#include <asm/uaccess.h>
#include <asm/io.h>

#define   MINOR_DEV_NAME        "minordev"
#define   MINOR_DEV_MAJOR            240
#define   MINOR_WRITE_ADDR        0x0378
#define   MINOR_READ_ADDR         0x0379

int minor0_open (struct inode *inode, struct file *filp)
{
    printk( "call minor0_open\n" );
    return 0;
}

ssize_t minor0_write (struct file *filp, const char *buf, size_t count, loff_t *f_pos)
{
    unsigned char status;    
    int           loop;

    for( loop = 0; loop < count; loop++ )
    {
        get_user( status, (char *) buf ); 
        outb( status , MINOR_WRITE_ADDR );   
    }    
    return count;
}

int minor0_release (struct inode *inode, struct file *filp)
{
    printk( "call minor0_release\n" );    
    return 0;
}

int minor1_open (struct inode *inode, struct file *filp)
{
    printk( "call minor1_open\n" );
    return 0;
}

ssize_t minor1_read(struct file *filp, char *buf, size_t count, loff_t *f_pos)
{
    unsigned char status;
    int           loop;
    
    for( loop = 0; loop < count; loop++ )
    {
        status = inb( MINOR_READ_ADDR );
        put_user( status, (char *) &buf[loop] );
    }

    return count;
}

int minor1_release (struct inode *inode, struct file *filp)
{
    printk( "call minor1_release\n" );
    return 0;
}

struct file_operations minor0_fops =
{
    .owner    = THIS_MODULE,
    .write    = minor0_write,
    .open     = minor0_open,
    .release  = minor0_release,
};

struct file_operations minor1_fops =
{
    .owner    = THIS_MODULE,
    .read     = minor1_read,
    .open     = minor1_open,
    .release  = minor1_release,
};

int minor_open (struct inode *inode, struct file *filp)
{
    printk( "call minor_open\n" );
    switch (MINOR(inode->i_rdev)) 
    {
    case 1: filp->f_op = &minor0_fops; break;
    case 2: filp->f_op = &minor1_fops; break;
    default : return -ENXIO;
    }
    
    if (filp->f_op && filp->f_op->open)
        return filp->f_op->open(inode,filp);
        
    return 0;
}

struct file_operations minor_fops =
{
    .owner    = THIS_MODULE,
    .open     = minor_open,     
};

int minor_init(void)
{
    int result;

    result = register_chrdev( MINOR_DEV_MAJOR, MINOR_DEV_NAME, &minor_fops);
    if (result < 0) return result;

    return 0;
}

void minor_exit(void)
{
    unregister_chrdev( MINOR_DEV_MAJOR, MINOR_DEV_NAME );
}

module_init(minor_init);
module_exit(minor_exit);

MODULE_LICENSE("Dual BSD/GPL");


~~~

## makefile

~~~
obj-m	:= minor_dev.o

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