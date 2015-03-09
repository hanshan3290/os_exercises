#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别：
 
UEFI比BIOS少了两层引导记录，它在所有平台上提供一致的启动服务。除此之外，UEFI具有可信启动流程，通过签名进行认证，使得其安全性高于BIOS。

 2. 描述PXE的大致启动流程：
 
首先开机并做自我测试，然后与服务器建立连接，下载内核镜像，然后客户端就可以根据下载的文件启动机器。


## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 
运行自检程序后装入主引导记录，然后活动分区的引导扇区被装入内存，NTLDR从引导扇区被装入并初始化，将处理器的实模式改为32位平滑内存模式。NTLDR开始运行适当的小文件系统驱动程序，读boot.ini文件后装载所选操作系统，运行Ntdetect。Ntdetect搜索硬件。然后NTLDR装载Ntoskrnl.exe，Hal.dll和系统信息集合并把控制权交给Ntoskrnl.exe，这时,启动程序结束,装载阶段开始。

 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常。
 
1. 中断：键盘输入，鼠标输入，硬件的接入，磁盘的读写。   2. 异常：除法计算的除零错，访问无权限的存储空间。

 2. Linux的系统调用有哪些？大致的功能分类有哪些？    
 
Linux系统调用的数目约为200个，大致分类为：进程控制，文件系统控制，系统控制，内存管理，网络管理，socket管理，用户管理，进程间通信等。
 
 3. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？ 
 
ucore的系统调用共22个，为exit,fork,wait,exec,yield,kill,getpid,putc,pgdir,gettime,lab6_set_priority,sleep,open,close,read,write,seek,fstat,fsync,getcwd,getdirentry,dup。大致可分为进程管理，内存管理，文件操作等类别。

 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)
 

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。并说明`int`、`iret`、`call`和`ret`的指令准确功能

代码编写时候，函数调用使用call等调用库函数，系统调用使用int等调用系统函数。
执行时候，系统调用需要调用系统的内核服务，而函数调用不用进入内核。
int的功能是进入系统调用，iret的功能是系统调用结束后返回。call的功能是调用函数，ret的功能是函数调用完以后返回。
