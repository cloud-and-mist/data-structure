# 数据结构
## 线性表
### 由同类型数据元素构成有序序列的线性结构
- 表中元素个数：线性表长度
- 线性表没有元素：空表
- 表起始位置：表头，表结束位置：表尾
- 线性表可以用含有一个元素和一个指针的链表来表示
## 广义表
- 广义表是线性表的推广
- 对于线性表而言，n个元素都是基本的单元素
- 广义表中，这些元素不仅可以是单元素也可以是另一个广义表。
- 广义表是复杂的链表：里面包含了指向一个线性表的指针，一个元素，和指向下一个广义表的指针等等......
- union 可以把不同类型的数据结合在一起
```c
typedef struct GNode *GList;
struct GNode{
    int Tag;
    /*标志域：0表示结点为单元素，1表示结点为广义表*/
    union{/*子表指针域SubList与单元素数据域Data复用，公用存储空间*/
        ElementType Data;
        GList SubList;
    }URegion;
    GList Next;//指向后继节点
}
```
## 多重链表
### 链表中的节点可能隶属于多个链
- 多重链表中结点的指针域会有多个，如前面例子包含了Next和SubList两个指针域
- 包含两个指针域的链表不一定是多重链表：如双向链表
### 多种链表广泛用途
- 基本上如树、图这样相对复杂的数据结构都可以采用多重链表方式进行存储

## 堆栈
### 后缀表达式
- 62/3-42*+=6/2-3+4*2=8
- 计算策略：从左往右扫描，逐个处理运算数和运算符号
- 启示：需要有种存储方法：顺序存储运算数，在需要时倒序输出。
### 堆栈：具有一定操作约束的线性表
- 只在一端（栈顶，Top）做插入，删除
- 插入数据：入栈Push
- 删除数据：出栈Pop
- 后入先出————类似拿堆在一起的碗
### 堆栈的链式存储实现
- 堆栈是一个有序的序列：只在一头做操作，但链表有两个头————关键是找到应该在链表的哪一头进行入栈出栈操作
- 应该在链表的head出进行入栈出栈操作
### 堆栈删除
```c
ElementType Pop(Stack S)
{
    struct SNode *FirstCell;
    ElementType TopElem;
    if(IsEmpty(S))
    {
        printf("堆栈空");
        return NULL;
    }
    else
    {
        FirstCell = S->Next;
        /*S是指向堆栈头结点的指针，S中并没有存储数据*/
        /*删除结点需要把这个结点原先申请的空间删除
        引入一个新的指针指向这个结点，最后将这个新的指针free了就可以了*/
        S->Next = FirstCell ->Next;
        TopElem = FirstCell ->Element;
        free(FirstCell);
        FirstCell = NULL;
        /*需不需要这一步让这个指针消失呢？free是将这个指针指向的内存空间消失了
        但这个指针还存着这个地址，让指针等于NULL 将其啊变成一个没有指向地址的空指针*/
        return TopElem;
    }
}
```
### 栈的应用
- 函数调用
- 表达式求值【中缀表达式转后缀表达式，用堆栈算法可以写一个科学计算器——百度搜索源码并理解其运行】
- [科学计算器](https://blog.csdn.net/qq_41799219/article/details/79942825)

## 队列
### 具有一定操作约束的线性表
- 插入与删除操作：只能在一端插入，而在另一端删除
- 数据插入：入队列(AddQ)
- 数据删除：出队列(DeleteQ)
- 先来先服务
- 先进先出：FILO
### 静态队列————用数组实现，且静态队列必须是循环队列
### 链式队列————用链表实现
### 循环队列
- 利用求余函数来实现循环
### 队列的链式存储
- 链表头做删除操作，链表尾做插入操作
## 串【只能存储字符】
### 串的顺序存储结构【用得更多】
```c
#define MAXLEN 255
typedef struct{
    char ch[MAXLEN+1];
    /*存储串的一维数组
    一般ch[0]不放字符，从ch[1]开始放*/
    int length;//串得当前长度
}
```
### 串的链式存储结构——块链
```c
#define CHUNKSIZE 80 //块的大小可由用户定义
typedef struct Chunk{
    char ch[CHUNKSIZE];
    struct Chunk *next;
}Chunk;

typedef struct{
    Chunk *head,*tail;//串的头指针和尾指针
    int curlen;//串的当前长度
}LString;//字符串的块链结构
```
### 串的模式匹配算法
- 算法目的：确定主串中所含子串（模式串）第一次出现的位置（定位）
- 算法应用：搜索引擎，拼写检查
- 算法种类：1.BF算法（暴力破解） 2.KMP算法（速度快）
```c
int Index_BF(SString S,SString T,int pos){
    int i=pos,j=1;//pos表示从某处开始进行查找
    while(i<=S.length && j>=T.length){
        if(S.ch[i]==T.ch[j])
        {
            i++;
            j++;
        }//主串和子串依次匹配下一个字符
        else
        {
            i=i-j+2;//主串的回溯【画图可知】
            j=1
        }//主串，子串指针回溯重新开始下依次匹配
    }
    if(j>=T.length)
    return i-T.length;//返回匹配的第一个字符下标
    else
    return 0;//模式匹配不成功
}
```
