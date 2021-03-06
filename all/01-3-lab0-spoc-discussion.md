# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- 可以读懂: )

>  

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- 从硬件层的图上来看，我对中断控制器的工作原理和其能完成的功能不够了解。

>   

请给出你觉得的中断的作用是什么？使用中断有何利弊？
- 中断即停止正在运行的程序。利：增加对程序的控制；可以实现多程序同时运行。弊：增加编程难度，增加出错的概率。

>   

哪些困难（请分优先级）会阻碍你自主完成lab实验？
- （优先级从高到低）1. 对实验原理理解不透彻 2. 缺乏编程技巧和调试经验

>   

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- 1、首先需要用gcc(g++) 对源文件进行编译生成可执行文件，并且在编译时加上选项-g，把调试信息加到目标文件中。2、假设生成的可执行文件为test，那么gdb test 可以用gdb打开test文件，然后通过break linenum设置断点。可以输入list查看源文件和行号，方便设置断点。断点设置好后就可以run命令运行到断点处了。
>   

了解函数调用栈对lab实验有何帮助？
- 帮助调试

>   

你希望从lab中学到什么知识？
- 1. 理解基本原理：操作系统对CPU，内存，外设的管理；操作系统对应用程序的控制等 2. 掌握一些基本的编程技巧，体会原理和实践之间的联系以及差异。

>   

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- 无。

> 

熟悉基本的git命令行操作命令，从github上的 http://www.github.com/chyyuu/ucore_lab 下载ucore lab实验

> 

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1

> 

对于如下的代码段，请说明”：“后面的数字是什么含义
```
 /* Gate descriptors for interrupts and traps */
 struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
 };
 ```

- 位域：即占用二进制位的位数。

> 

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？

- 65538

> 

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
-程序如下。
```
#include <stdio.h>
#include "list.h"


int main(int argc, const char * argv[])
{
    list_entry_t HS0,HS1,HS2,HS3;
    
    list_init(&HS0);
    printf("%d %d %d\n",HS0.prev, HS0.next, &HS0);
    
    HS0.prev = NULL;
    HS0.next = &HS1;
    HS1.prev = &HS0;
    HS1.next = &HS2;
    HS2.prev = &HS1;
    HS2.next = NULL;

    list_add(&HS1, &HS3);

    printf("%d %d %d\n",HS0.prev, HS0.next, &HS0);
    printf("%d %d %d\n",HS1.prev, HS1.next, &HS1);
    printf("%d %d %d\n",HS3.prev, HS3.next, &HS3);
    printf("%d %d %d\n",HS2.prev, HS2.next, &HS2);
    
    
    list_del(&HS3);
    printf("%d %d %d\n",HS0.prev, HS0.next, &HS0);
    printf("%d %d %d\n",HS1.prev, HS1.next, &HS1);
    printf("%d %d %d\n",HS2.prev, HS2.next, &HS2);
}
```

> 

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  

---

