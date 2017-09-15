---
title: Linux /proc/$pid 进程状态详解
date: 2017-03-29 10:23:35
tags: Linux 
---
查看Linux运行中的进程状态。

查看运行中的程序的详情，需要用到程序的Pid，可以使用top获取pid

## aux
/proc/[pid]/auxv包含传递给进程的ELF解释器信息，格式是每一项都是一个unsigned long长度的ID加上一个unsigned long长度的值。最后一项以连续的两个0x00开头。举例如下：

`# hexdump -x /proc/pid/auxv `

## cmline
/proc/[pid]/cmdline是一个只读文件，包含进程的完整命令行信息。如果这个进程是zombie进程，则这个文件没有任何内容。举例如下：

`cat /proc/2948/cmdline`

/usr/sbin/libvirtd--listen

<!-- more -->

## comm
/proc/[pid]/comm包含进程的命令名。举例如下：

`cat /proc/2948/comm`

libvirtd

## cwd
/proc/[pid]/cwd是进程当前工作目录的符号链接。举例如下：

`ls -lt /proc/2948/cwd`

lrwxrwxrwx 1 root root 0 Nov  9 12:14 /proc/2948/cwd -> /

## environ
/proc/[pid]/environ显示进程的环境变量。举例如下：

`strings /proc/2948/environ`

LANG=POSIX
LC_CTYPE=en_US.UTF-8
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NOTIFY_SOCKET=@/org/freedesktop/systemd1/notify
LIBVIRTD_CONFIG=/etc/libvirt/libvirtd.conf
LIBVIRTD_ARGS=--listen
LIBVIRTD_NOFILES_LIMIT=2048

## exe
/proc/[pid]/exe为实际运行程序的符号链接。举例如下：

`ls -lt /proc/2948/exe`

lrwxrwxrwx 1 root root 0 Nov  5 13:04 /proc/2948/exe -> /usr/sbin/libvirtd

## fd
/proc/[pid]/fd是一个目录，包含进程打开文件的情况。举例如下：

`ls -lt /proc/3801/fd`

total 0
lrwx------. 1 root root 64 Apr 18 16:51 0 -> socket:[37445]
lrwx------. 1 root root 64 Apr 18 16:51 1 -> socket:[37446]
lrwx------. 1 root root 64 Apr 18 16:51 10 -> socket:[31729]
lrwx------. 1 root root 64 Apr 18 16:51 11 -> socket:[34562]
l-wx------. 1 root root 64 Apr 13 16:35 2 -> /root/.vnc/localhost.localdomain:1.log

目录中的每一项都是一个符号链接，指向打开的文件，数字则代表文件描述符。

## latency
/proc/[pid]/latency显示哪些代码造成的延时比较大（使用这个feature，需要执行“echo 1 > /proc/sys/kernel/latencytop”）。举例如下：

`cat /proc/2948/latency`

Latency Top version : v0.1
30667 10650491 4891 poll_schedule_timeout do_sys_poll SyS_poll system_call_fastpath 0x7f636573dc1d
8 105 44 futex_wait_queue_me futex_wait do_futex SyS_futex system_call_fastpath 0x7f6365a167bc

每一行前三个数字分别是后面代码执行的次数，总共执行延迟时间（单位是微秒）和最长执行延迟时间（单位是微秒），后面则是代码完整的调用栈。

## limits

/proc/[pid]/limits显示当前进程的资源限制。举例如下：

`cat /proc/2948/limits`

| Limit  | Soft Limit| Hard Limit| Units|
|:-------|-----:|:-----|:----:|
| Max cpu time| unlimited| unlimited   | seconds |
| Max file size | unlimited| unlimited   | bytes |
| Max data size | unlimited| unlimited   | bytes |
| Max stack size | 8388608| unlimited   | bytes |

Soft Limit表示kernel设置给资源的值，Hard Limit表示Soft Limit的上限，而Units则为计量单元。

## maps

/proc/[pid]/maps显示进程的内存区域映射信息。举例如下：

`cat /proc/2948/maps`

其中注意的一点是[stack:<tid>]是线程的堆栈信息，对应于/proc/[pid]/task/[tid]/路径。

## root

/proc/[pid]/stack显示当前进程的内核调用栈信息，只有内核编译时打开了CONFIG_STACKTRACE编译选项，才会生成这个文件。举例如下：

`cat /proc/2948/stack`

## statm

/proc/[pid]/statm显示进程所占用内存大小的统计信息，包含七个值，度量单位是page（page大小可通过getconf PAGESIZE得到）。举例如下：

`cat /proc/2948/statm `

72362 12945 4876 569 0 24665 0

各个值含义：

a) 进程占用的总的内存；

b）进程当前时刻占用的物理内存；

c）同其它进程共享的内存；

d）进程的代码段；

e）共享库（从2.6版本起，这个值为0）；

f）进程的堆栈；

g）dirty pages（从2.6版本起，这个值为0）。

## syscall

/proc/[pid]/syscall显示当前进程正在执行的系统调用。举例如下：

`cat /proc/2948/syscall`

7 0x7f4a452cbe70 0xb 0x1388 0xffffffffffdff000 0x7f4a4274a750 0x0 0x7ffd1a8033f0 0x7f4a41ff2c1d

第一个值是系统调用号（7代表poll），后面跟着6个系统调用的参数值（位于寄存器中），最后两个值依次是堆栈指针和指令计数器的值。如果当前进程虽然阻塞，但阻塞函数并不是系统调用，则系统调用号的值为-1，后面只有堆栈指针和指令计数器的值。如果进程没有阻塞，则这个文件只有一个“running”的字符串。

内核编译时打开了CONFIG_HAVE_ARCH_TRACEHOOK编译选项，才会生成这个文件。

## wchan

/proc/[pid]/wchan显示当进程sleep时，kernel当前运行的函数。举例如下：

`cat /proc/2948/wchan`

kauditd_


