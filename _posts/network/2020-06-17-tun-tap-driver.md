---
layout: single
title:  "tun/tap device Drivers"
tags: 
    - Linux
---

根据参考文章, 理解 "利用 Linux tap/tun 虚拟设备写一个 ICMP echo 程序"

## 理解 ICMP echo程序
ICMP echo即 ping的 server端实现.

流程是, open() tun设备, 注册为 `IFF_TUN   - TUN device (no Ethernet headers)`, 这样收到的就是纯 IP Packet,
方便在 网络层做 ping客户端的程序的 echo. 

因为 echo server只要在 IP Packet header中交换 src于 dst, 使用了 memcpy()进行swap, 使用 write()把 IP Packet写入该字符设备, 
tun/tap driver就在用户空间发送 IP Packet给 dst IP

## 源码

```c
// [利用 Linux tap/tun 虚拟设备写一个 ICMP echo 程序](https://cloud.tencent.com/developer/article/1432454)
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>
#include <net/if.h>
#include <sys/ioctl.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
#include <sys/types.h>
#include <linux/if_tun.h>

int tun_alloc(char *dev, int flags)
{
    assert(dev != NULL);

    struct ifreq ifr;
    int fd, err;

    char *clonedev = "/dev/net/tun";

    if ((fd = open(clonedev, O_RDWR)) < 0) {
        return fd;
    }

    memset(&ifr, 0, sizeof(ifr));
    ifr.ifr_flags = flags;
    
    if (*dev != '\0') {
        strncpy(ifr.ifr_name, dev, IFNAMSIZ);
    }
    if ((err = ioctl(fd, TUNSETIFF, (void *) &ifr)) < 0) {
        close(fd);
        return err;
    }

    // 一旦设备开启成功，系统会给设备分配一个名称，对于tun设备，一般为tunX，X为从0开始的编号；
    // 对于tap设备，一般为tapX
    strcpy(dev, ifr.ifr_name);

    return fd;
}

int main()
{
    int tun_fd, nread;
    char buffer[4096];
    char tun_name[IFNAMSIZ];

    tun_name[0] = '\0';

    /* Flags: IFF_TUN   - TUN device (no Ethernet headers)
     *        IFF_TAP   - TAP device
     *        IFF_NO_PI - Do not provide packet information
     */
    tun_fd = tun_alloc(tun_name, IFF_TUN | IFF_NO_PI);

    if (tun_fd < 0) {
        perror("Allocating interface");
        exit(1);
    }

    printf("Open tun/tap device: %s for reading...\n", tun_name);
    
    while (1) {
        unsigned char ip[4];
        // 收包
        nread = read(tun_fd, buffer, sizeof(buffer));
        if (nread < 0) {
            perror("Reading from interface");
            close(tun_fd);
            exit(1);
        }
        
        printf("Read %d bytes from tun/tap device\n", nread);
        
        // 简单对收到的包调换一下顺序
        memcpy(ip, &buffer[12], 4);
        memcpy(&buffer[12], &buffer[16], 4);
        memcpy(&buffer[16], ip, 4);

        buffer[20] = 0;
        *((unsigned short *)&buffer[22]) += 8;
        
        // 发包
        nread = write(tun_fd, buffer, nread);

        printf("Write %d bytes to tun/tap device, that's %s\n", nread, buffer);
    }
    return 0;
}
```

## 参考文章

[Universal TUN/TAP device driver](https://github.com/torvalds/linux/blob/master/Documentation/networking/tuntap.rst)

https://github.com/torvalds/linux/blob/master/include/linux/if_tun.h

https://github.com/torvalds/linux/blob/master/drivers/net/tun.c

[[原创] 详解云计算网络底层技术——虚拟网络设备 tap/tun 原理解析](https://www.cnblogs.com/bakari/p/10450711.html)

[Linux虚拟网络设备之tun/tap](https://segmentfault.com/a/1190000009249039)

[Tun/Tap interface tutorial](https://backreference.org/2010/03/26/tuntap-interface-tutorial/)

[TUN/TAP - Wikipedia](https://en.wikipedia.org/wiki/TUN/TAP)

[利用 Linux tap/tun 虚拟设备写一个 ICMP echo 程序](https://cloud.tencent.com/developer/article/1432454)

Linux设备驱动(3rd Ed) - 第三章: 字符驱动