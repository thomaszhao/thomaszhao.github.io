---
title: 使用systemtap获取.ko文件
tagline: ""
category : kernel
layout: post
tags : [systemtap, ko, stap, xxd]
---
{% include JB/setup %}

有些产品是以内核模块`*.ko`形式存在的，使用`insmod`命令插入内核，
Linux同时也提供了`init_module(2)`函数。

前几天碰到一个友商的产品，将`.ko`文件隐藏在可执行文件中，没有直接提供明文的`.ko`文件，
而是在运行时调用`init_module`函数注册。

问题来了，怎么提取这种情况的`.ko`文件？答案当然是`systemtap`。

<!-- more -->


思路
-----------------------------------------------------------

1. `init_module`函数的参数中有完整的`.ko`文件的`char数组`，使用systemtap可以抓取到`init_module`函数的参数；
2. 似乎systemtap不能直接写文件，那么就把参数(char数组)以明文(16进制hex)形式打印出来；
3. 再将16进制的明文转成char写到文件中（正好有`xxd`命令，省了写代码了），就形成了.ko文件。


使用systemtap获取.ko文件
------------------------------------------------------------

### 准备stp脚本：

root@thomas:systemtap# cat probe_init_module.stp

```
#!/usr/bin/stap -g -DMAXACTION=400000

probe begin
{
         log("Thomas: begin to probe")
}

probe syscall.init_module {
    printf ("%s(%d) init_module (%s)\n", execname(), pid(), argstr)
    printf ("count=%d\n", $len)

    for(i=0; i < $len; i++) {
     printf("%02x ", user_uint8($umod + i))
     if(i && (i+1)%16 == 0) {
         printf("\n")
     }
    }
    printf("\n")
    exit()
}

probe end
{
         log("Thomas: end to probe")
}
```

### 获取`.ko`文件

执行`stap`命令：

```
#/usr/bin/stap -g -DMAXACTION=400000  probe_init_module.stp -o 1.txt
```

执行会调用`init_module`的命令：

    # ./<command> start

去除前两行信息和最后一行的log信息。

使用`xxd`命令反向把hex变为文件：

    #xxd -r -p 1.txt <xxx>.ko

### 检查`.ko`文件的正确性：

    # ls -l <xxx>.ko     ## 检查大小是否和参数一致；
    # file ./<xxx>.ko    ## 检查文件格式是否正确；
    # modinfo ./<xxx>.ko    ## 检查版本等信息是否正确；

### 使用`strace`命令跟踪模块的参数

    # strace ./<command> start
    ## 查找其中`init_module`函数的参数

### 使用insmod将模块插入：

    # insmod ./<xxx>.ko <other parameters>


内核面前本无秘密，大功高成~

下一步就打开`IDA pro`尽情的反吧。


