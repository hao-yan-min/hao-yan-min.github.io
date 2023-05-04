---
title: cache实验1
date: 2023-05-02 19:16:39
tags: 
- 计算机系统结构
- cache
- 硬件
categories:
- 华科实验
---





## 实验内容

在csim.c提供的程序框架中，编写实现一个Cache模拟器。



### 实现功能

输入：内存访问轨迹（src\traces子文件夹中的*.trace文件）。 操作：模拟缓存相对内存访问轨迹的命中（hit）/缺失行为（miss）。 输出：命中、缺失和（缓存行）脱胎/驱除（基于LRU算法）的总数。 完成csim.c文件的结果能够使用命令行参数产生下面的输出结果。 输出形式如下：

 ```bash 
 linux> ./csim -s 4 -E 2 -b 4 -t traces/yi.trace
 ```

输出：

```bash 
hits:4 misses:5 evictions:2
```

 即当输入时，你完成的csim.c输出和上述相同的功能(\*.trace文件下面有详细信息)。



### getopt函数

getopt函数可以读取命令行的指令，该函数有以下的几个具体的参数：

* **int argc**：argc是指令的行数，该参数就是main函数的入口参数int argc。
* **char\* const argv[]**：该参数就是main函数的入口参数argv
* **const char* command_option**：该参数定义了命令行指令的解析方式。对于一个命令行，一些字母表示该指令的命令行选项。有些选型没有参数，则这些选项可以写在一起。有些选项后面带参数，则参数用``:``表示。有些参数可以为空，则用``::``表示。例如：``hvs:E:b:t:``该参数表示命令行选项-h -v为可选选型，不需要参数。-E,-b,-t则分别带一个参数。

在具体使用时，可以使用while循环。getopt()函数会返回一个字符表示当前读取的命令行选项。如果该选项之后有参数，则该参数会被保存到一个叫``optarg``的全局变量中。

当指令被读完之后，函数会返回-1。



### LRU策略

LUR策略是指将最久没有使用过的块替换出去的策略。在本次实验中，可以在每一个块中设置一个count的值。每次访存的时候，如果没有访问该块，则该块的count就加一，否则如果访问到了该块或该块发生了替换，则将count置为0。



### 数据结构

定义每一行line的结构为：

```c
typedef struct
{
    int valid;
    uint64_t tag;
    uint64_t counter;  //记录
}line;
```

每一组set就指向一个line数组，cache则指向一个set的数组：
```c
typedef line* Set;
typedef Set* Cache;
```



### educoder查看编译信息

educoder的测试程序没有提供编译信息的查看，因此debug十分不方便，不过我们可以在命令行手动进行编译。具体步骤如下：

```bash
cd /data/workspace/myshixun/
vim Makefile
```

手动创建一个Makefile文件。先输入``i``然后复制粘贴以下代码：

```makefile
#
# Student makefile for Cache Lab
# Note: requires a 64-bit x86-64 system 
#
CC = gcc
CFLAGS = -g -Wall -Werror -std=c99 -m64

all: csim test-trans tracegen
	# Generate a handin tar file each time you compile
	-tar -cvf ${USER}-handin.tar  csim.c trans.c 

csim: csim.c cachelab.c cachelab.h
	$(CC) $(CFLAGS) -o csim csim.c cachelab.c -lm 

test-trans: test-trans.c trans.o cachelab.c cachelab.h
	$(CC) $(CFLAGS) -o test-trans test-trans.c cachelab.c trans.o 

tracegen: tracegen.c trans.o cachelab.c
	$(CC) $(CFLAGS) -O0 -o tracegen tracegen.c trans.o cachelab.c

trans.o: trans.c
	$(CC) $(CFLAGS) -O0 -c trans.c

#
# Clean the src dirctory
#
clean:
	rm -rf *.o
	rm -f *.tar
	rm -f csim
	rm -f test-trans tracegen
	rm -f trace.all trace.f*
	rm -f .csim_results .marker

```

之后``Esc``退出编辑模式，``:wq``保存编辑。

最后在命令行输入：``make``就可以查看编译情况了。如果出现了simulator error那么可以是程序解析命令行参数的部分出错了。



具体代码如下：

```c
#include "cachelab.h"
#include <stdio.h>	
#include <stdint.h>	
#include <unistd.h> 
#include <getopt.h> 
#include <stdlib.h> 
#include <errno.h>  

typedef struct
{
    int valid;
    uint64_t tag;
    uint64_t counter;  //记录
}line;

typedef line* Set;
typedef Set* Cache;
Cache initCache(uint64_t S,uint64_t E);
void search_cache(uint64_t tag,uint64_t E,Set set,int* hit,int* mis,int* eviction,int verbose);
void printhelp();
void printhelp()
{
    printf("Usage: ./csim [-hv] -s <num> -E <num> -b <num> -t <file>\n");
	printf("Options:\n");						 					 
	printf("-h			Print this help message.\n");						                		 
	printf("-v			Optional verbose flag.\n");                   		 
	printf("-s <num>	Number of set index bits.\n");                   		 
	printf("-E <num>	Number of lines per set.\n");                   		 
	printf("-t <file>	Trace file.\n");                   		 
    printf("\n");                   		 
	printf("Examples:\n");						 
	printf("linux> ./csim -s 4 -E 1 -b 4 -t traces/yi.trace\n");						
	printf("linux> ./csim -v -s 8 -E 2 -b 4 -t traces/yi.trace\n");
    return;		
}
void ReleaseMemory(Cache cache, uint64_t S, uint64_t E)
{
    for (uint64_t i = 0; i < S; ++i)
    {
        free(cache[i]);
    }
    free(cache);
}
Cache initCache(uint64_t S,uint64_t E)  //生成S组set,每组set有E个line。
{
    Cache cache=calloc(S,sizeof(Set));
    for(int i=0;i<S;i++)
    {
        cache[i]=calloc(E,sizeof(line));
    }
    return cache;

}

void search_cache(uint64_t tag,uint64_t E,Set set,int* hit,int* mis,int* eviction,int verbose)
{
    int flg=0;
    for(uint64_t i=0;i<E;i++)
    {
        if(set[i].tag==tag&&set[i].valid==1)
        {
            if(verbose==1)
            {
                printf("hit ");
            }
            *hit=*hit+1;
            set[i].counter=0;
            flg=1;
        }
        set[i].counter++;
    }
    if(flg==1) return;
    //如果没有命中
    if(verbose==1)
    {
        printf("miss ");
    }
    *mis=*mis+1;
    for(uint64_t i=0;i<E;i++)
    {
        if(set[i].valid==0)
        {
            set[i].tag=tag;
            set[i].valid=1;
            set[i].counter=0;
            return;
        }
    }
    //需要替换
    uint64_t old=UINT64_MAX;
    uint64_t old_index=0;
    for(uint64_t i=0;i<E;i++)
    {
        if(set[i].counter<old)
        {
            old_index=i;
        }
    }
    //进行替换
    set[old_index].tag=tag;
    set[old_index].counter=0;
    set[old_index].valid=1;
    *eviction=*eviction+1;
    printf("and eviction ");
    return;
}
int main(int argc, char * const argv[])
{
    int hit=0,mis=0,eviction=0;
    uint64_t s=0,E=0,b=0,S=0;
    char op;
    int verbose=0;
    FILE *fp=NULL;
    while((op=getopt(argc,argv,"hvs:E:b:t:"))!=-1)  
    {
        switch (op)
        {
        case 'h':
        {
            printf("Usage: ./csim [-hv] -s <num> -E <num> -b <num> -t <file>\n");
	        printf("Options:\n");						 					 
	        printf("-h			Print this help message.\n");						                		 
	        printf("-v			Optional verbose flag.\n");                   		 
	        printf("-s <num>	Number of set index bits.\n");                   		 
	        printf("-E <num>	Number of lines per set.\n");                   		 
	        printf("-t <file>	Trace file.\n");                   		 
            printf("\n");                   		 
	        printf("Examples:\n");						 
	        printf("linux> ./csim -s 4 -E 1 -b 4 -t traces/yi.trace\n");						
	        printf("linux> ./csim -v -s 8 -E 2 -b 4 -t traces/yi.trace\n");
            return 0;		
        }
        case 'v':
        {
            verbose=1;
            break;
        }
        case 's':
        {
            if(atol(optarg)<=0) return -1;
            s=atol(optarg);
            S=1 << s;
            break;
        }
        case 'E':
        {
            if(atol(optarg)<=0) return -1;
            E=atol(optarg);
            break;
        }
        case 'b':
        {
            if(atol(optarg)<=0) return -1;
            b=atol(optarg);
            break;
        }
        case 't':
        {
            if((fp=fopen(optarg,"r"))==NULL) return -1;
            break;
        }
        default:
            break;
        }
        //参数保存在一个名为optarg的全局变量中，可以使用atol函数将字符串转换成整数。
    }
    Cache cache=initCache(S,E);
    int sz=0;
    uint64_t address=0;
    while((fscanf(fp,"%c %lx %d",&op,&address,&sz))!=-1)
    {
        if(op=='I')
        {
            continue;
        }
        else
        {
            uint64_t tag=address >> b;
            uint64_t mask=1 << s;
            mask=mask-1;
            uint64_t index_set=(address >> b) & mask;
            tag=tag >> s;
            Set set=cache[index_set];
            if(op=='L'||op=='S')
            {
                if(verbose==1)
                {
                    printf("%c %lx,%d ",op,address,sz);
                }
                search_cache(tag,E,set,&hit,&mis,&eviction,verbose);
            }
            else if (op=='M')
            {
                if(verbose==1)
                {
                    printf("%c %lx,%d ",op,address,sz);
                }
                search_cache(tag,E,set,&hit,&mis,&eviction,verbose);
                search_cache(tag,E,set,&hit,&mis,&eviction,verbose);
            }
        }
        printf("\n");
    }
    ReleaseMemory(cache,S,E);
    printSummary(hit, mis, eviction);
    return 0;
}

```



