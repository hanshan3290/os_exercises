# lab1 SPOC思考题

## 个人思考题

NOTICE
- 有"w2l2"标记的题是助教要提交到学堂在线上的。
- 有"w2l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。
---

请描述ucore OS配置和驱动外设时钟的准备工作包括哪些步骤？ (w2l2)

- 包括对IDT的初始化，将中断服务程序的地址写入IDT。
- 函数pic_init(void)完成了对8259中断控制器的初始化。包括对interrupts,master,Vector offset,slave的设置。
- 函数clock_init(void)完成了对8253时钟外设的初始化。将ticks置为0后enable IRQ_TIMER 
>  

lab1中完成了对哪些外设的访问？ (w2l2)

- lab1完成了对时钟，串口，并口，CGA，键盘的访问。

>  

lab1中的cprintf函数最终通过哪些外设完成了对字符串的输出？ (w2l2)

- cprintf函数用到的3个外设:串口，并口，CGA。函数如下：
- lpt_putc - copy console output to parallel port
- cga_putc - print character to console
- serial_putc - print character to serial port

>  

---

## 小组思考题

---

lab1中printfmt函数用到了可变参，请参考写一个小的linux应用程序，完成实现定义和调用一个可变参数的函数。(spoc)
```
#include <stdio.h>
#include <stdarg.h>
#define fnAdd(...) _fnAdd(0,__VA_ARGS__)
int _fnAdd(int start,...)
{
    va_list arg_ptr;
    int sum=0,t=0;
    va_start(arg_ptr,start);
    t=va_arg(arg_ptr,int);
    while(t)
    {
        sum+=t;
        t=va_arg(arg_ptr,int);
    }
    va_end(arg_ptr);
    return sum;
}
int main()
{
    printf("%d\n",fnAdd(1,2,3,0));
    printf("%d\n",fnAdd(1,2,3,5,0));
    return 0;
}
```



如果让你来一个阶段一个阶段地从零开始完整实现lab1（不是现在的填空考方式），你的实现步骤是什么？（比如先实现一个可显示字符串的bootloader（描述一下要实现的关键步骤和需要注意的事项），再实现一个可加载ELF格式文件的bootloader（再描述一下进一步要实现的关键步骤和需要注意的事项）...） (spoc)
- 先实现一个可显示字符串的bootloader，然后实现实模式到保护模式的切换，实现段机制，最后实现一个可加载ELF格式文件的bootloader，就可以加载操作系统并执行了。

> 


如何能获取一个系统调用的调用次数信息？如何可以获取所有系统调用的调用次数信息？请简要说明可能的思路。(spoc)
- 对每一个系统调用设置一个计数器，每次调用时候计数器加一。当用户需要获取一个系统调用的次数信息的时候，输出其中一个，要获取所有的次数信息时，输出总和。

> 

如何修改lab1, 实现一个可显示字符串"THU LAB1"且依然能够正确加载ucore OS的bootloader？如果不能完成实现，请说明理由。
- [x]  

> 

对于ucore_lab中的labcodes/lab1，我们知道如果在qemu中执行，可能会出现各种稀奇古怪的问题，比如reboot，死机，黑屏等等。请通过qemu的分析功能来动态分析并回答lab1是如何执行并最终为什么会出现这种情况？
- [x]  

> 

对于ucore_lab中的labcodes/lab1,如果出现了reboot，死机，黑屏等现象，请思考设计有效的调试方法来分析常在现背后的原因。
- [x]  

> 

---

## 开放思考题

---

如何修改lab1, 实现在出现除零错误异常时显示一个字符串的异常服务例程的lab1？
- [x]  

> 


在lab1/bin目录下，通过`objcopy -O binary kernel kernel.bin`可以把elf格式的ucore kernel转变成体积更小巧的binary格式的ucore kernel。为此，需要如何修改lab1的bootloader, 能够实现正确加载binary格式的ucore OS？ (hard)
- [x]  

>

GRUB是一个通用的bootloader，被用于加载多种操作系统。如果放弃lab1的bootloader，采用GRUB来加载ucore OS，请问需要如何修改lab1, 能够实现此需求？ (hard)
- [x]  

>


如果没有中断，操作系统设计会有哪些问题或困难？在这种情况下，能否完成对外设驱动和对进程的切换等操作系统核心功能？
- [x]  

>  

---

## 各知识点的小练习

### 4.1 启动顺序
---
读入ucore内核的代码？

- [x]  

> 

跳转到ucore内核的代码？

- [x]  

> 

全局描述符表的初始化代码？

- [x]  

> 

GDT内容的设置格式？初始映射的基址和长度？特权级的设置位置？

- [x]  

> 

可执行文件格式elf的各个段的数据结构？

- [x]  

> 

如果ucore内核的elf是否要求连续存放？为什么？

- [x]  

> 
---

### 4.2 C函数调用的实现
---

函数调用的stackframe结构？函数调用的参数传递方法有哪几种？
- [x]  

> 

系统调用的stackframe结构？系统调用的参数传递方法有哪几种？

- [x]  

> 
---

### 4.3 GCC内联汇编
---

使用内联汇编的原因？

- [x]  

> 特权指令、性能优化

对ucore中的一段内联汇编进行完整的解释？

- [x]  

> 
---

### 4.4 x86中断处理过程
---

4.4 x86中断处理过程

中断描述符表IDT的结构？

- [x]  

> 

中断描述表到中断服务例程的地址计算过程？

- [x]  

> 

中断处理中硬件压栈内容？用户态中断和内核态中断的硬件压栈有什么不同？

- [x]  

> 

中断处理中硬件保存了哪些寄存器？

- [x]  

> 
---

### 4.5 练习一 ucore编译过程
---

gcc编译、ld链接和dd生成两个映像对应的makefile脚本行？

- [x]  

> 
---

### 4.6 练习二 qemu和gdb的使用
---

qemu的命令行参数含义解释？

- [x]  

> 

gdb命令格式？反汇编、运行、断点设置

- [x]  

> 
---

### 练习三 加载程序
---

A20的使能代码分析？

- [x]  

> 

生成主引导扇区的过程分析？

- [x]  

> 

保护模式的切换代码？

- [x]  

> 

如何识别elf格式？对应代码分析？

- [x]  

> 

跳转到elf的代码？

- [x]  

> 
函数调用栈获取？

- [x]  

> 
---

### 4.8 练习四和五 ucore内核映像加载和函数调用栈分析
---

如何识别elf格式？对应代码分析？

- [x]  

> 

跳转到elf的代码？

- [x]  

> 

函数调用栈获取？
- [x]  

> 
---


### 4.9 练习六 完善中断初始化和处理
---

各种设备的中断初始化？
- [x]  

> 
中断描述符表IDT的排列顺序？
- [x]  

> 中断号
CPU加电初始化后中断是使能的吗？为什么？

- [x]  

> 
中断服务例程的入口地址在什么地方设置的？

- [x]  

> 
alltrap的中断号是在哪写入到trapframe结构中的？

- [x]  

> 
trapframe结构？
- [x]  

> 
---
