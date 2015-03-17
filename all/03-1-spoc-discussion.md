# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- 最先匹配：简单，在高地址有大块空闲分区。   但是会留下一些外部碎片，并且分配较大块时很慢。
- 最优匹配：可以避免大块空间被切分，减少外部碎片，简单。    但是外部碎片会比较小，难以被再次利用，释放会比较复杂。
- 最差匹配：中等大小分配时效果很好，避免过多的小碎片。    但是会产生外部碎片，大的区域会被切分，释放比较复杂。
- 伙伴系统：简单，外部碎片较少。    但是会产生内部碎片，合并有条件故导致一些连续区域不可合并。

>  

## 小组思考题

请参考ucore lab2代码，采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)来实现，给出你的设思路，并给出测试用例。 (spoc)

- 最先匹配维护一个列表，设置flag标记每一块是否被占用。
- malloc顺序查找，改变标记。
- free先改变标记，然后合并相邻空闲块。
- 地址对齐：对于申请的大小，求出大于等于它的最小的4的倍数作为malloc的参数。

```
//2012013290 - 最先匹配

#include <iostream>
using namespace std;

struct page
{
    int pageSize;
    int start,end;
    bool isfree;
    page *next;
};

struct pmm_manager
{
    page *pmm;
    void init();
    page *alloc_pages(int size);
    void free_pages(page *p);
    void show();
};

void pmm_manager::init()
{
    this->pmm=new page;
    pmm->pageSize=1024;
    pmm->start=0;
    pmm->end=1024;
    pmm->isfree=true;
    pmm->next=NULL;
}

page *pmm_manager::alloc_pages(int size)
{
    while(size%4!=0)
    {
        size++;
    }
    page *p=this->pmm, *q;
    while(p)
    {
        if(p->isfree&&p->pageSize>size)
        {
            q=new page;
            q->pageSize=p->pageSize-size;
            q->start=p->start+size;
            q->end=p->end;
            q->isfree=true;
            q->next=p->next;
            
            p->pageSize=size;
            p->end=q->start;
            p->next=q;
            p->isfree=false;
            break;
        }
        else
        {
            p=p->next;
        }
    }
    return p;
}

void pmm_manager::free_pages(page *p)
{
    p->isfree=true;
    page *m=this->pmm, *n;
    while(m->next)
    {
        n=m->next;
        if(m->isfree && n->isfree)
        {
            m->pageSize+=n->pageSize;
            m->end=n->end;
            m->next=n->next;
            free(n);
        }
        m=m->next;
    }
}

void pmm_manager::show()
{
    cout<<"*************************"<<endl;
    page *p=this->pmm;
    while(p)
    {
        cout<<"|| "<<p->start<<"~"<<p->end<<" ||"<<"---"<<p->isfree<<endl;
        p=p->next;
    }
    cout<<"*************************"<<endl;
}
int main()
{
    pmm_manager manage;
    page *n1,*n2,*n3,*n4,*n5,*n6;
    manage.init();
    n1=manage.alloc_pages(98);
    n2=manage.alloc_pages(39);
    n3=manage.alloc_pages(60);
    n4=manage.alloc_pages(80);
    n5=manage.alloc_pages(20);
    n6=manage.alloc_pages(100);
    manage.show();
    
    manage.free_pages(n2);
    manage.show();
    manage.free_pages(n3);
    manage.show();
    n2=manage.alloc_pages(80);
    manage.show();
    n3=manage.alloc_pages(100);
    manage.show();
    return 0;
}
```

--- 

## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。

## “连续内存分配”与视频相关的课堂练习

### 5.1 计算机体系结构和内存层次
MMU的工作机理？

- [x]  

>  http://en.wikipedia.org/wiki/Memory_management_unit

L1和L2高速缓存有什么区别？

- [x]  

>  http://superuser.com/questions/196143/where-exactly-l1-l2-and-l3-caches-located-in-computer
>  Where exactly L1, L2 and L3 Caches located in computer?

>  http://en.wikipedia.org/wiki/CPU_cache
>  CPU cache

### 5.2 地址空间和地址生成
编译、链接和加载的过程了解？

- [x]  

>  

动态链接如何使用？

- [x]  

>  


### 5.3 连续内存分配
什么是内碎片、外碎片？

- [x]  

>  

为什么最先匹配会越用越慢？

- [x]  

>  

为什么最差匹配会的外碎片少？

- [x]  

>  

在几种算法中分区释放后的合并处理如何做？

- [x]  

>  

### 5.4 碎片整理
一个处于等待状态的进程被对换到外存（对换等待状态）后，等待事件出现了。操作系统需要如何响应？

- [x]  

>  

### 5.5 伙伴系统
伙伴系统的空闲块如何组织？

- [x]  

>  

伙伴系统的内存分配流程？

- [x]  

>  

伙伴系统的内存回收流程？

- [x]  

>  

struct list_entry是如何把数据元素组织成链表的？

- [x]  

>  



