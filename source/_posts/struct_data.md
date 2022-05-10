title: 数据结构（C）

---

首先声明：本篇文章仅仅是个人笔记记录与拓展，并非个人原创，主要参考了B站UP主TyranLucifer的数据结构教程

[课程参考](https://www.bilibili.com/video/BV1W64y1z7jh?spm_id_from=333.999.0.0 )

[UP主源代码](https://tyrantlucifer.com )

# 链表

## 单链表

```C
#include<stdio.h>
#include<stdlib.h>

typedef struct Node
{
    int data;     //数据域
    struct Node* next; //指针域
}Node;

/**
 * @brief 链表初始化
 *
 * @return Node*
 */
Node* Create()
{
    Node* L = (Node*)malloc(sizeof(Node));
    L->next = NULL; //初始化为NULL
    L->data = 0; //开始节点数为0（不包括头结点）
    return L;
}

/**
 * @brief 头插法
 *
 * @param L 表头
 * @param data 插入值
 */
void HeadInser(Node* L, int data)
{
    Node* Y = (Node*)malloc(sizeof(Node));
    Y->data = data;
    Y->next = L->next;//新的节点指向头结点指向的那一个节点
    L->next = Y;  //头结点指向新的节点
    L->data++;//头结点数增加
}


/**
 * @brief 尾插法
 *
 * @param L 表头
 * @param data 插入值
 */

void TailInser(Node* L, int data)
{
    Node* Y = (Node*)malloc(sizeof(Node));
    Node* LY = L;
    //无论是否为空 都要实现的部分放外面可减少代码量
    Y->data = data;
    //如果L为空
    if (!L)
    {
        Y->next = L->next;//新节点Y指向头结点指向的下一个
        L->next = Y;//头结点指向Y
    }
    else
    {
        while (L->next)//如果不为空的时候找到最后一个节点
        {
            L = L->next;
        }
        Y->next = L->next;
        L->next = Y;
    }
    LY->data++;
}
/**
 * @brief 打印链表L
 *
 * @param L
 */
void PrintList(Node* L)
{
    Node* Y = L;
    Y = Y->next;//不打印表头
    while (Y)
    {
        printf("%d -> ", Y->data);
        Y = Y->next;
    }
    printf("NULL\n");
}

/**
 * @brief 查找链表中是否有对于值
 *
 * @param L
 * @param data 需要查找值
 */
void List_Find(Node* L, int data)
{
    Node* Y = L->next;//不查找表头
    while (Y)
    {
        if (Y->data == data)
        {
            printf("找到了：%d\n", Y->data);
            return;
        }
        Y = Y->next;
    }
    printf("抱歉，链表中没有查询值\n");
}

/**
 * @brief 删除链表所以符合值得节点 若链表为空则删除头结点
 *
 * @param L
 * @param data 需要删除的值
 */
void List_Delet(Node* L, int data)
{
    Node* Y = L;
    Node* f = L;
    if (Y->next == NULL)//空链表
    {
        free(Y);
        Y = NULL;
        return;
    }
    else
    {
        while (Y->next)
        {
            if (Y->next->data == data)
            {
                Node* P = Y->next;//记住删除的位置
                Y->next = Y->next->next;//指向下下个结点
                printf("删除值为%d后的链表: ", P->data);
                free(P);
                f->data--;
                return;
            }
            Y = Y->next;
        }
    }
    L->data--;
    printf("没有找到需要删除的结点\n");
}
/**
 * @brief 销毁链表
 *
 * @param L
 */
void List_Destroy(Node* LY)
{
    Node* n = LY->next;
    Node* L = LY->next;
    while (L)
    {
        L = L->next;
        free(n);
        LY->data--;
        n = L;
    }
    free(LY);
}
int main(void)
{
    Node* LY = Create();

    HeadInser(LY, 1);
    HeadInser(LY, 2);
    HeadInser(LY, 3);
    TailInser(LY, 4);
    TailInser(LY, 5);
    TailInser(LY, 6);
    PrintList(LY);
    printf("节点数为：%d\n", LY->data);
    List_Find(LY, 0);
    List_Find(LY, 1);
    List_Delet(LY, 6);
    PrintList(LY);
    List_Destroy(LY);
    LY = NULL;
    return 0;
}

```

## 单循环链表

```C

#include<stdio.h>
#include<stdlib.h>

typedef struct Node
{
    int data;
    struct Node* next;
}Node;
/**
 * @brief 来拿表初始化
 *
 * @return Node*
 */
Node* InitList()
{
    Node* L = (Node*)malloc(sizeof(Node));
    L->data = 0;
    L->next = L; //指向自己构成循环
    return L;
}

/**
 * @brief 头插法
 *
 * @param L 需要插入节点的链表
 * @param data 插入节点值
 */
void HeadInsert(Node* L, int data)
{
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data;
    node->next = L->next;  //指向第一个节点
    L->next = node; //头结点指向新的节点
    L->data++;
}

/**
 * @brief 尾插法
 *
 * @param L
 * @param data
 */
void TailInsert(Node* L, int data)
{
    Node* nl = L;
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data;
    while (nl->next != L)//找到最后一个节点
    {
        nl = nl->next;
    }
    node->next = L;
    nl->next = node;
    L->data++;
}
/**
 * @brief 打印链表
 *
 * @param L
 */
void PrintList(Node* L)
{
    Node* node = L->next;
    while (node != L)
    {
        printf("%d->", node->data);
        node = node->next;
    }
    printf("NULL\n");
}
/**
 * @brief 删除data对应值得节点
 *
 * @param L  检索的链表
 * @param data 删除第一个符合值得节点
 * @return int
 */
int Delete(Node* L, int data)
{
    Node* pre = L; //记住删除节点的前一个节点
    Node* node = L->next;
    while (node != L)
    {
        if (node->data == data)
        {
            pre->next = node->next; //需删除节点的前一个节点指向删除节点后一个节点
            free(node);
            L->data--;
            printf("删除首个值为%d的节点\n", data);
            return 1;
        }
        pre = node;
        node = node->next;
    }
    printf("删除节点失败");
    return 0;

}

void DestroyList(Node* L)
{
    Node* n = L->next;
    Node* LY = L->next;
    while (L != LY)
    {
        LY = LY->next; //L指向下一个节点再删除
        free(n);
        L->data--;
        n = LY;
    }
    free(L);
    printf("链表销毁！！");
}

int main()
{
    Node* L = InitList();
    HeadInsert(L, 3);
    HeadInsert(L, 2);
    HeadInsert(L, 1);
    TailInsert(L, 4);
    TailInsert(L, 5);
    TailInsert(L, 6);
    PrintList(L);
    Delete(L, 6);
    PrintList(L);
    DestroyList(L);
    L = NULL;
    return 0;
}
```

## 双链表

```C
#include <stdio.h>
#include <stdlib.h>


typedef struct Node
{
    int data; //数据域(头结点用来计节点个数)
    struct Node* pre; //指向前一个的指针
    struct Node* next; //指向后一个的指针
}Node;

/**
 * @brief 初始化链表：建立头结点，并且将其返回
 *
 * @return Node*
 */
Node* InitList()
{
    Node* L = (Node*)malloc(sizeof(Node));
    L->data = 0;
    L->pre = NULL;
    L->next = NULL;
    return L;
}
/**
 * @brief 头插法：建立一个新节点，插入在原本链表的头结点和第一个节点之间
 *
 * @param L    插入的链表
 * @param data 插入值
 */
void HeadInser(Node* L, int data)
{
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data; //填充数据域
    node->next = L->next; //新的节点指向第一个节点（头结点的下一个）
    node->pre = L; //新节点的前一个指向头结点

    //如果链表不止头结点
    if (L->next)
    {
        /*这里注意到 L->next是指向旧的第一个节点的，先对旧的第一个节点的前
        * 进行了操作，然后在改变L->next指向新的第一个节点
         */
        L->next->pre = node; //原初的第一个节点的前指向新的第一个节点
        L->next = node; //头结点指向node，即新的第一个节点
    }
    //如果链表只有头结点
    else
    {
        L->next = node;
    }
    L->data++;
}
/**
 * @brief 尾插法：新的节点插入到链表末尾
 *
 * @param L 需要插入节点的链表
 * @param data  插入的节点的值
 */
void TailInsert(Node* L, int data)
{
    Node* node = L; //对node指针操作，目的是不改动L指针
    Node* nl = (Node*)malloc(sizeof(Node));
    nl->data = data; //填充数据
    while (node->next) //node找到最后一个节点位置
    {
        node = node->next;
    }
    nl->next = node->next; //新的节点指向最后一个节点的后
    node->next = nl; //旧的最后一个节点的后指向新的节点
    nl->pre = node; //新的前直接指向旧的最后一个节点
    L->data++;
}
/**
 * @brief 删除传入值得节点
 *
 * @param L 删除节点的链表
 * @param data 删除的值
 * @return int 1：成功找到对应节点且删除  0：删除失败
 */
int Delete(Node* L, int data)
{
    Node* node = L->next;
    while (node)
    {
        if (node->data == data) //如果节点值与插入值相同
        {
            node->pre->next = node->next; //删除节点的前一个节点的后指向删除节点的后一个
            if (node->next) //如果删除节点不是最后一个节点
            {
                node->next->pre = node->pre; //删除节点的下一个的前指向删除节点的前一个节点
            }
            L->data--;
            free(node);
            return 1; //匹配且删除成功
        }
        node = node->next;
    }
    return 0; //删除值匹配失败
}
/**
 * @brief 打印链表
 *
 * @param L 需要打印的链表
 */
void PrintList(Node* L)
{
    Node* node = L->next;
    while (node)
    {
        printf("%d->", node->data);
        node = node->next;
    }
    printf("NULL\n");
}
/**
 * @brief 销毁链表
 *
 * @param L 需要销毁的链表
 */
void DestroyList(Node* L)
{
    Node *n = L->next;
    Node* LY = L->next;
    while (LY)
    {
        LY = LY->next;
        free(n);
        L->data--;
        n = LY;
    }
    free(L);
}

int main()
{
    Node* L = InitList(); //建立链表
    HeadInser(L, 3);
    HeadInser(L, 2);
    HeadInser(L, 1);
    TailInsert(L, 4);
    TailInsert(L, 5);
    TailInsert(L, 6);
    PrintList(L);
    Delete(L, 5);
    PrintList(L);
    DestroyList(L);
    L = NULL;
    return 0;
}
```

## 双循环链表

```C
#include <stdio.h>
#include <stdlib.h>


typedef struct Node
{
    int data; //数据域(头结点用来计节点个数)
    struct Node* pre; //指向前一个的指针
    struct Node* next; //指向后一个的指针
}Node;

/**
 * @brief 初始化链表：建立头结点，并且将其返回
 *
 * @return Node*
 */
Node* InitList()
{
    Node* L = (Node*)malloc(sizeof(Node));
    L->data = 0;
    L->pre = L;
    L->next = L;
    return L;
}
/**
 * @brief 头插法：建立一个新节点，插入在原本链表的头结点和第一个节点之间
 *
 * @param L    插入的链表
 * @param data 插入值
 */
void HeadInser(Node* L, int data)
{
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data; //填充数据域
    node->next = L->next; //新的节点指向第一个节点（头结点的下一个）
    node->pre = L; //新节点的前一个指向头结点
    L->next->pre = node; //原本第一个节点的前指向node（这里不用考虑是否只是一个包含头结点的链表，因为它是循环的。
                         //假设只有头结点，那么它的下一个的前还是它自己，并不会指向不明地方）
    L->next = node; //头结点的下一个指向新节点
    L->data++;
}
/**
 * @brief 尾插法：新的节点插入到链表末尾
 *
 * @param L 需要插入节点的链表
 * @param data  插入的节点的值
 */
void TailInsert(Node* L, int data)
{
    Node* node = L; //对node指针操作，目的是不改动L指针
    Node* nl = (Node*)malloc(sizeof(Node));
    nl->data = data; //填充数据
    while (node->next != L) //node找到最后一个节点位置
    {
        node = node->next;
    }
    nl->pre = node; //新节点的前指向旧的最后一个节点
    nl->next = L; //新的节点的后指向头结点
    L->pre = nl; //头结点的前指向新节点
    node->next = nl; //旧的最后一个节点指向新插入末尾的节点
    nl->pre = node; //新的前直接指向旧的最后一个节点
    L->data++;
}
/**
 * @brief 删除传入值得节点
 *
 * @param L 删除节点的链表
 * @param data 删除的值
 * @return int 1：成功找到对应节点且删除  0：删除失败
 */
int Delete(Node* L, int data)
{
    Node* node = L->next;
    while (node != L)
    {
        if (node->data == data) //如果节点值与插入值相同
        {
            node->pre->next = node->next; //删除节点前一个节点的后指向删除节点的后一个节点
            node->next->pre = node->pre; //删除节点的有一个节点的前指向删除节点的前一个节点
            L->data--;
            free(node);
            return 1; //匹配且删除成功
        }
        node = node->next;
    }
    return 0; //删除值匹配失败
}
/**
 * @brief 打印链表
 *
 * @param L 需要打印的链表
 */
void PrintList(Node* L)
{
    Node* node = L->next;
    while (node != L)
    {
        printf("%d->", node->data);
        node = node->next;
    }
    printf("NULL\n");
}
/**
 * @brief 销毁链表
 *
 * @param L 需要销毁的链表
 */
void DestroyList(Node* L)
{
    Node* LY = L->next;
    Node* n = L->next;
    while (L != LY)
    {
        LY = LY->next;
        free(n);
        L->data--;
        n = LY;
    }
    free(L);//销毁表头
}

int main()
{
    Node* L = InitList(); //建立链表
    HeadInser(L, 3);
    HeadInser(L, 2);
    HeadInser(L, 1);
    TailInsert(L, 4);
    TailInsert(L, 5);
    TailInsert(L, 6);
    PrintList(L);
    Delete(L, 5);
    PrintList(L);
    DestroyList(L);
    L = NULL;
    return 0;
}
```

# 栈



```C
#include<stdio.h>
#include<stdlib.h>
typedef struct Node
{
    int data;
    struct Node *next;
}Node;

/**
 * @brief 栈初始化
 * 
 * @return Node* 
 */
Node *InitStack()
{
    Node *L = (Node*)malloc(sizeof(Node));
    L->data = 0;
    L->next = NULL;
    return L;
}
/**
 * @brief 压栈，出去头结点外，将值压入栈中，类似单链表头插法
 * 
 * @param L 
 * @param data 
 */
void Push(Node* L, int data)
{
    Node *node = (Node*)malloc(sizeof(Node));
    node->data = data;
    node->next = L->next;
    L->next = node;
    L->data++;
}
/**
 * @brief 弹出栈顶
 * 
 * @param L 
 * @return int 返回站定制 
 */
int Pop(Node *L)
{
    if(L->data == 0) //如果栈空
    {
        printf("栈为空\n");
        return 0;
    }
    else
    {
        Node *node = L->next;
        int data = node->data;
        L->next = node->next; //类似于单链表中的删除某个节点
        free(node);
        L->data--;
        return data;
    }
}
/**
 * @brief 栈是否为空
 * 
 * @param L 
 * @return int 0：非空 1：空
 */
int IsEmpty(Node *L)
{
    if(L->data == 0 || L->next == NULL)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
/**
 * @brief 清空栈（并没销毁，保留了头结点,当然也可以连头结点都删，只不过下述没有给出对应代码）
 * 
 */
void ClearStack(Node *stack)
{
    Node *node = stack->next;
    Node *LY = stack->next;
    while(LY)
    {
        LY = LY->next;
        free(node);
        stack->data--; 
        node = LY;
    }
}

/**
 * @brief 打印栈中元素
 * 
 * @param stack 
 */
void PrintStack(Node *stack)
{
    Node *node = stack->next;
    while(node)
    {
        printf("%d->", node->data);
        node = node->next;
    }
    printf("NULL\n");
}

int main()
{
    Node *stack = InitStack(); //创建栈
    Push(stack, 5);
    Push(stack, 4);
    Push(stack, 3);
    Push(stack, 2);
    Push(stack, 1);
    PrintStack(stack);
    printf("栈顶值为:%d\n", Pop(stack));
    PrintStack(stack);
    printf("清空前栈的长度：%d\n", stack->data);
    ClearStack(stack);
    printf("清空后栈的长度：%d\n",stack->data);
    /*
     仍然可以对stack使用，并未销毁表头        
    */
    return 0;
}
```

# 队列

## 非循环队列

```C

/*
* 队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，
  而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。队列中没有元素时，称为空队列。
*/
#include<stdio.h>
#include<stdlib.h>

typedef struct Node
{
    int data;
    struct Node* next;
    struct Node* pre;
}Node;
/**
 * @brief 队的初始化
 *
 * @return Node*
 */
Node* InitQueue()
{
    Node* Q = (Node*)malloc(sizeof(Node));
    Q->data = 0;
    Q->pre = Q;
    Q->next = Q;
    return Q;
}
/**
 * @brief 入队操作
 *
 * @param Q    需要插入值的队
 * @param data 插入值
 */
void EnQueue(Node* Q, int data)
{
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data;
    node->next = Q; //新节点的后指向头结点
    node->pre = Q->pre; //新节点的前指向最后一个节点
    Q->pre->next = node; //旧的最后一个节点的后指向新的节点
    Q->pre = node; //头结点的前指向最后一个节点
    Q->data++;
}
/**
 * @brief 判断是否为空队
 *
 * @param Q 判断的队列
 * @return int 1：空       0：非空
 */
int IsEmpty(Node* Q)
{
    if (Q->data == 0 || Q->next == Q)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
/**
 * @brief 出队操作
 *
 * @param Q
 * @return int 返回出队值
 */
int DeQueue(Node* Q)
{
    if (IsEmpty(Q))
    {
        printf("error,队空!!!");
        return 0;
    }
    else
    {
        Node* LY = Q;
        Node* node = Q->next;
        Q->next = Q->next->next; //先改变头结点的next指到第二个节点
        Q->next->pre = Q; //操作第二个节点的前指向头结点（相当于沙成黏糊了）
        int data = node->data;
        free(node);
        LY->data--; //出队后队长自减
        return data;
    }
}
/**
 * @brief 销毁队
 *
 * @param Q
 */
void DestroyQueue(Node* Q)
{
    Node* node = Q;
    Node* LY = Q;
    while (Q != LY)
    {
        Q = Q->next;
        free(node);
        node = Q;
    }
}

void PrintQueue(Node* Q)
{
    Node* node = Q->next;
    while (node != Q)
    {
        printf("%d->", node->data);
        node = node->next;
    }
    printf("NULL\n");
}

int main()
{
    Node* Q = InitQueue();
    EnQueue(Q, 5);
    EnQueue(Q, 4);
    EnQueue(Q, 3);
    EnQueue(Q, 2);
    EnQueue(Q, 1);
    PrintQueue(Q);
    printf("队长: %d\n", Q->data);
    printf("出队：%d\n", DeQueue(Q));
    printf("出队：%d\n", DeQueue(Q));
    printf("出队：%d\n", DeQueue(Q));
    printf("队长: %d\n", Q->data);
    PrintQueue(Q);
    DestroyQueue(Q);
    return 0;
}
```

## 循环队列

```C
/**
* 循环队列就是将队列存储空间的最后一个位置绕到第一个位置
* ，形成逻辑上的环状空间，供队列循环使用。在循环队列结构中
* ，当存储空间的最后一个位置已被使用而再要进入队运算时，
* 只需要存储空间的第一个位置空闲，便可将元素加入到第一个位置，
* 即将存储空间的第一个位置作为队尾。 [1]  循环队列可以更简单防止伪溢出的发生，但队列大小是固定的。
**/



#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 5

typedef struct Queue
{
    int front; //队首位置
    int rear; //队尾位置
    int data[MAXSIZE];
}Queue;
/**
 * @brief 循环队初始化
 * 
 * @return Queue* 
 */
Queue* InitQueue()
{
    Queue *Q = (Queue*)malloc(sizeof(Queue));
    Q->front = 0;
    Q->rear = 0;
    return Q;
}
/**
 * @brief 判断队是否满了
 * 
 * @param Q 
 * @return int 0：没满 1：满了
 */
int IsFull(Queue *Q)
{   
    /*
    若满了尾巴+1对最大长度取余永远等于头
    */
    if((Q->rear + 1) % MAXSIZE == Q->front)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
/**
 * @brief 是否空队
 * 
 * @param Q 
 * @return int 0：否 1：是
 */
int IsEmpty(Queue *Q)
{
    if(Q->front == Q->rear)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}

/**
 * @brief 获取队长
 * 
 * @param Q 
 * @return int  返回队长值
 */
int GetQueLen(Queue *Q)
{
    /*
    若队尾在的位置大于队首 则MAXSIZE不起作用，即队长为队尾减去队首
    反之，队尾减去队首所得值得绝对值为空的个数，加上MAXSIZE即为队长
    */
    int len = (Q->rear - Q->front + MAXSIZE) % MAXSIZE;
    return len;
}
/**
 * @brief 入队（最后一个位置留空）
 * 
 * @param Q 
 * @param data 入队的值 
 * @return int 1：成功 0：队已满
 */
int EnQueue(Queue *Q, int data)
{
    if(IsFull(Q))
    {
        printf("抱歉，队已满。");
        return 0;
    }
    else
    {
        Q->data[Q->rear] = data;
        Q->rear = (Q->rear + 1) % MAXSIZE;//若刚刚已经指向了最后一个位置下标，再往后则跳到第一个位置下标
        return 1;
    }
}
/**
 * @brief 删除队首
 * 
 * @param Q 
 * @return int 返回队首值
 */
int DeQueue(Queue *Q)
{
    if(IsEmpty(Q))
    {
        printf("队已空\n");
        return -1;
    }
    else
    {
        int data = Q->data[Q->front];
        Q->front = (Q->front + 1) % MAXSIZE; //同上理
        return data;
    }
}

/**
 * @brief 打印列表
 * 
 * @param Q 
 */
void PrintQueue(Queue *Q)
{
    int len = GetQueLen(Q);
    int index = Q->front;
    int i = 0;
    for(i = 0;i < len; ++i)
    {
        printf("%d->", Q->data[index]);
        index = (index + 1) % MAXSIZE; //若超出队尾则又循环到队头
    }
    printf("NLUU\n");
}


int main()
{
    Queue *Q = InitQueue();
    EnQueue(Q, 4);
    EnQueue(Q, 3);
    EnQueue(Q, 2);
    EnQueue(Q, 1);
    PrintQueue(Q);
    printf("队长:%d\n",GetQueLen(Q));
    EnQueue(Q, 5);
    printf("删除队首：%d\n", DeQueue(Q));
    printf("删除队首：%d\n", DeQueue(Q));
    PrintQueue(Q);
    return 0;
}
```

# 字符串匹配

## 直接匹配

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct String
{
    char* data;
    int len;
}String;
/**
 * @brief 字符串初始化
 *
 * @return String*
 */
String* InitString()
{
    String* s = (String*)malloc(sizeof(String));
    s->data = NULL; //初始化为空指针
    s->len = 0;
    return s;
}

/**
 * @brief 给字符串赋值
 *
 * @param s 需要赋值的字符串
 * @param data 赋值的字符串
 */
void StringAssign(String* s, char* data)
{
    if (s->data) //若本身不为空释放掉在重新开辟
    {
        free(s->data);
    }
    int len = 0;
    char* temp = data;
    while (*temp) //获取data 长度
    {
        len++;
        temp++;
    }
    if (len == 0)
    {
        s->data = NULL;
        s->len = 0;
    }
    else
    {
        temp = data;
        s->len = len;
        s->data = (char*)malloc(sizeof(char) * (len + 1));//还有个空字符
        int i = 0;
        for (i = 0; i < len; i++, temp++)
        {
            s->data[i] = *temp;
        }
        s->data[len] = '\0'; //字符串末尾记得加上'\0'
    }
}
/**
 * @brief 字符串输出
 *
 * @param s 所需要输出的字符串
 */
void PrintString(String* s)
{
    int i = 0;
    for (i = 0; i < s->len; i++)
    {
        printf(i == 0 ? "%c" : "->%c", s->data[i]);//第一个字符则直接输出，其它字符在前面加上->
    }
    printf("\n");
}

/**
 * @brief 强制匹配字符串
 *
 * @param master 主字符串
 * @param sub 子字符串
 */
void ForceMatch(String* master, String* sub)
{
    int i = 0;
    int j = 0;
    while (i < master->len && j < sub->len)
    {
        if (master->data[i] == sub->data[j])
        {
            i++;
            j++;
        }
        else
        {
            i = i - j + 1; //相当于每次匹配失败主字符串往后挪一个
            j = 0;
        }
    }
    if (j == sub->len)
    {
        printf("匹配成功!!\n");
    }
    else
    {
        printf("匹配失败\n");
    }
}

/**
* 下面使用了命令行参数传参
**/
int main(int argc, char* argv[])
{
    String* s1 = InitString();
    String* s2 = InitString();
    StringAssign(s1, argv[1]);
    StringAssign(s2, argv[2]);
    // 不适用命令行参数
    //StringAssign(s1, "abcd");
    //StringAssign(s2, "abcde");
    PrintString(s1);
    PrintString(s2);
    ForceMatch(s1, s2);
    return 0;
}
```

## KMP算法实现

[KMP算法参考链接](https://blog.csdn.net/woshidenghaitao/article/details/89439921 )

```C
/*
* 主要思路为：前缀后缀 例如给定字符串为ABCDABD              0 0 0 0 1 2 1  前缀后缀：例如第一个A前面有一个A故为1  后面AB 对应前面AB故第二个B为2 最后一个D同理前缀后缀一个D 故为1
* 它的next数组（将前缀后缀往右移动一个单位，且首个变为-1）为-1 0 0 0 0 1 2
*/


#include <stdio.h>
#include <stdlib.h>

typedef struct String
{
    char* data;
    int len;
}String;
/**
 * @brief 字符串初始化
 *
 * @return String*
 */
String* InitString()
{
    String* s = (String*)malloc(sizeof(String));
    s->data = NULL; //初始化为空指针
    s->len = 0;
    return s;
}

/**
 * @brief 给字符串赋值
 *
 * @param s 需要赋值的字符串
 * @param data 赋值的字符串
 */
void StringAssign(String* s, char* data)
{
    if (s->data) //若本身不为空释放掉在重新开辟
    {
        free(s->data);
    }
    int len = 0;
    char* temp = data;
    while (*temp) //获取data 长度
    {
        len++;
        temp++;
    }
    if (len == 0)
    {
        s->data = NULL;
        s->len = 0;
    }
    else
    {
        temp = data;
        s->len = len;
        s->data = (char*)malloc(sizeof(char) * (len + 1));//还有个空字符
        int i = 0;
        for (i = 0; i < len; i++, temp++)
        {
            s->data[i] = *temp;
        }
        s->data[len] = '\0'; //字符串末尾记得加上'\0'
    }
}
/**
 * @brief 字符串输出
 *
 * @param s 所需要输出的字符串
 */
void PrintString(String* s)
{
    int i = 0;
    for (i = 0; i < s->len; i++)
    {
        printf("%c  ", s->data[i]);//第一个字符则直接输出，其它字符在前面加上->
    }
    printf("\n");
}
/**
 * @brief 获取字符串的下个数组
 *
 * @param s
 * @return int*
 */
int* GetNext(String* s)
{
    int* next = (int*)malloc(sizeof(int) * s->len);
    int j = 0;
    int k = -1;
    next[j] = k;
    while (j < s->len - 1)
    {
        if (k == -1 || s->data[j] == s->data[k]) //若字符匹配或者k为-1
        {
            k++;
            j++;
            //优化前的kmp 
            //next[j] = k;
            
            //优化后的kmp
            if (s->data[j] != s->data[k])
            {
                next[j] = k;
            }
            else //形如abab时候 ab对不上时候后面也是ab铁定也是对不上，需要直接往后挪
            {
                next[j] = next[k];
            }
        }
        else
        {
            k = next[k];
        }
    }
    return next;
}
/**
 * @brief
 *
 * @param next
 * @param len
 */
void PrintNext(int* next, int len)
{
    int i = 0;
    for (i = 0; i < len; i++)
    {
        printf("%d  ", next[i]);
    }
    printf("\n");
}
/**
 * @brief KMP算法匹配
 * 
 * @param master 主字符串
 * @param sub    匹配的字符串
 * @param next   主字符串的next数组
 */
void KmpMatch(String* master, String* sub, int* next)
{
    int i = 0;
    int j = 0;
    while (i < master->len && j < sub->len)
    {
        if (j == -1 || master->data[i] == sub->data[j])
        {
            i++;
            j++;
        }
        else
        {
            j = next[j]; //字符不匹配时候直接从下一个长度更短的前缀后缀 
        }
    }
    if (j == sub->len)
    {
        printf("匹配成功!\n");
    }
    else
    {
        printf("匹配失败!\n");
    }
}

int main(int argc, char* argv[])
{
    String* s1 = InitString();
    String* s2 = InitString();
    // StringAssign(s1,argv[1]);
    // StringAssign(s2,argv[2]);
    StringAssign(s1, "absabdaabdabss");
    StringAssign(s2, "abdabs");
    PrintString(s1);
    PrintString(s2);
    int* next = GetNext(s1); //获取s1的next数组
    PrintNext(next, s1->len);
    KmpMatch(s1, s2, next);
    return 0;
}
```

# 二叉树

## 二叉树的创建

```C

//二叉树创建、遍历、销毁、深度

#include <stdio.h>
#include <stdlib.h>

typedef struct TreeNode
{
    char data;
    struct TreeNode *lchild; //左孩子
    struct TreeNode *rchild; //右孩子
}TreeNode;

/**
 * @brief 二叉树的创建(先序遍历创建)，注意：用二级指针原因是修改了一级指针的内容
 *        根左右的创建方式
 * @param T 二叉树
 * @param data 用于建立二叉树的字符串
 * @param index 用于在递归中记录当前第N个字符
 */
void CreateTree(TreeNode **T, char *data, int *index)
{
    char ch;
    ch = data[*index];
    *index += 1;
    if(ch == '#')
    {
        *T = NULL; //若为#则为空节点
    }
    else
    {
        *T = (TreeNode*)malloc(sizeof(TreeNode));
        (*T)->data = ch;
        //根据递归思路知道：它会先创建左边的遇到#然后递归回来在创建右边的
        CreateTree(&((*T)->lchild), data, index); //创建左子树
        CreateTree(&((*T)->rchild), data, index); //创建右子树
    }
}
/**
 * @brief 先序遍历 简言之：先办事->处理左孩子->处理右孩子 中->左->右
 * 
 * @param T 
 */
void PreOrder(TreeNode *T)
{
    if(T == NULL)
    {
        return;
    }
    else
    {
        printf("%c", T->data); //先打印
        PreOrder(T->lchild); //在打印左孩子
        PreOrder(T->rchild); //最后打印右孩子
    }
}
/**
 * @brief 中序遍历：简言之：先处理左孩子->办事->处理右孩子 左->中->右
 * 
 * @param T 
 */
void InOrder(TreeNode *T)
{
    if(T == NULL)
    {
        return;
    }
    else
    {
        InOrder(T->lchild); //打印左孩子
        printf("%c", T->data);
        InOrder(T->rchild); //最后打印右孩子
    }
}
/**
 * @brief 后续遍历：简言之：先处理左孩子->右孩子->办事  左->右->中
 * 
 * @param T 
 */
void PostOrder(TreeNode *T)
{
    if(T == NULL)
    {
        return;
    }
    else
    {
        PreOrder(T->lchild); //打印左孩子
        PreOrder(T->rchild); //最后打印右孩子
        printf("%c", T->data);
    }
}
/**
 * @brief 后续遍历销毁二叉树：先删除左端然后右端最后根节点
 * 
 * @param T 
 */
void DestroyTree(TreeNode* T)
{
    if(T == NULL)
    {
        printf("# ");
        return;
    }
    DestroyTree(T->lchild);
    DestroyTree(T->rchild);
    printf("%c ", T->data);
    free(T);
    T = NULL;
    return;
}
/**
 * @brief 求树的深度（最大值）
 * 
 * @param T 
 * @return int 
 */
int TreeDepth(TreeNode *T)
{
    if(T != NULL)
    {
        /*
        从左到右，找到最左，左中判断右是否还有更深的树
        */
        int left = TreeDepth(T->lchild); //先找到最左的节点然后往回跳，且执行那层递归的右若为空则可直接往回跳（因为那个已经是最左的节点，最深的地方）
        int right = TreeDepth(T->rchild);//同上理
        return left > right ? left + 1 : right + 1;
    }
    return 0;
}

int main(int argc, char *argv[])
{
    TreeNode *T;
    int index = 0;
    //修改一级指针传其地址上述用二级指针接收，传第一个命令行参数的字符串
    // CreateTree(&T, argv[1], &index); 
    // char *s = "AB##C##"; //根A 左B 右C
    // char *s = "ABC###D#E##"; //根A 左B 右C
    char *s = "ABC##D#E##F##";
    CreateTree(&T, s, &index);
    PreOrder(T);
    printf("\n");
    InOrder(T);
    printf("\n");
    PostOrder(T);
    printf("\n");
    printf("二叉树销毁如下:\n");
    printf("树的最大深度:%d\n",TreeDepth(T));
    DestroyTree(T);
    return 0;
}
```

## 二叉树的层次遍历

```C
//层次遍历：通过队先进先出来实现

#include <stdio.h>
#include <stdlib.h>

typedef struct TreeNode
{
    char data;
    struct TreeNode *lchild; //左孩子
    struct TreeNode *rchild; //右孩子
}TreeNode;

/**
 * @brief 用于层次遍历的队
 * 
 */
typedef struct QueueNode
{
    TreeNode *data;
    struct QueueNode* pre;
    struct QueueNode* next;
}QueueNode;


/**
 * @brief 二叉树的创建，注意：用二级指针原因是修改了一级指针的内容
 *        根左右的创建方式
 * @param T 二叉树
 * @param data 用于建立二叉树的字符串
 * @param index 用于在递归中记录当前第N个字符
 */
void CreateTree(TreeNode **T, char *data, int *index)
{
    char ch;
    ch = data[*index];
    *index += 1;
    if(ch == '#')
    {
        *T = NULL; //若为#则为空节点
    }
    else
    {
        *T = (TreeNode*)malloc(sizeof(TreeNode));
        (*T)->data = ch;
        //根据递归思路知道：它会先创建左边的遇到#然后递归回来在创建右边的
        CreateTree(&((*T)->lchild), data, index); //创建左子树
        CreateTree(&((*T)->rchild), data, index); //创建右子树
    }
}
/**
 * @brief 先序遍历 简言之：先办事->处理左孩子->处理右孩子 中->左->右
 * 
 * @param T 
 */
void PreOrder(TreeNode *T)
{
    if(T == NULL)
    {
        return;
    }
    else
    {
        printf("%c ", T->data); //先打印
        PreOrder(T->lchild); //在打印左孩子
        PreOrder(T->rchild); //最后打印右孩子
    }
}
/**
 * @brief 队的初始化
 * 
 * @return QueueNode* 
 */
QueueNode* InitQueue()
{
    QueueNode *Q = (QueueNode*)malloc(sizeof(QueueNode));
    Q->data = NULL;
    Q->next = Q;
    Q->pre = Q;
    return Q;
}
/**
 * @brief 入队
 * 
 * @param data 入队的树
 * @param Q    队列
 */
void EnQueue(TreeNode *data, QueueNode *Q)
{
    QueueNode *node = (QueueNode*)malloc(sizeof(QueueNode));
    node->data = data;
    node->pre = Q->pre; //新的节点前指向旧的最后一个
    node->next = Q; 
    Q->pre->next =  node;//队头结点的前的next就是之前最后一个，它的next指向新的node成了倒数第二个
    Q->pre = node; //队的前重新指向新的最后一个 也就是node
}

/**
 * @brief 队是否为空
 * 
 * @param Q 
 * @return int 0：非空 1：空
 */
int IsEmpty(QueueNode *Q)
{
    if(Q->next == Q)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}

/**
 * @brief 删除队中元素：即第一个节点
 * 
 * @param Q 
 * @return QueueNode* 
 */
QueueNode *DeQueue(QueueNode *Q)
{
    if(IsEmpty(Q))
    {
        return NULL;
    }
    else
    {
        QueueNode *node = Q->next;
        Q->next->next->pre = Q; //第二个节点前指向头结点
        Q->next = Q->next->next; //头结点指向第二个节点
        return node;
    }
}
/**
 * @brief 层次遍历 
 *        先将根节点入队，然后若该根节点非空则进入while循环
 *        首先另其出队且输出，然后判断其左孩子是否空若不空入队，右同样
 *        例如：ABC##D##EF##G## 先序遍历：A->B->C->D->E->F->G 当层次遍历时候
 *        首先是A即根节点，入队然后出队，输出A，判断其左右孩子，左为B不为空，入队，右孩子也不为空，入队
 *        下一轮循环B出队 输出B，然后C、D入队
 *        在下一轮E出队，输出，F、G入队...一次类推
 *        总言之：先入先出，先左后右
 * @param Q 
 * @param T 
 */
void LevelTraverse(QueueNode *Q, TreeNode *T)
{
    EnQueue(T, Q);
    while (!IsEmpty(Q))
    {
        QueueNode *node = DeQueue(Q);
        printf("%c ",node->data->data); //队的data指针域的数据域
        if(node->data->lchild)
        {
            EnQueue(node->data->lchild, Q);
        }
        if(node->data->rchild)
        {
            EnQueue(node->data->rchild, Q);
        }
    }
    
}

int main(int argc, char *argv[])
{
    TreeNode *T;
    int index = 0;
    QueueNode *Q = InitQueue();
    // CreateTree(&T, argv[1], &index);
    // char *s = "ABC###D#E##";
    char *s = "ABC##D##EF##G##";
    CreateTree(&T,s, &index);
    PreOrder(T); //先序遍历
    printf("\n");
    LevelTraverse(Q, T); //层次遍历
    printf("\n");
    return 0;
}
```

## 二叉树非递归遍历

```C
//二叉树非递归遍历

#include <stdio.h>
#include <stdlib.h>

typedef struct TreeNode
{
    char data;
    struct TreeNode *lchild;
    struct TreeNode *rchild;
    int flag; //标志符，用于非递归后序遍历：初始化为0，遍历过置1.
    /*
    flag的作用：因为后续遍历是左右中，但是每次压入栈中时候中都会先一步于右压入栈中，它是否弹出输出主要看它的右是否遍历过了，若遍历
    过了的话就直接弹出且输出。非递归后续遍历中会给出详细例子讲解
    */
}TreeNode;

typedef struct StackNode
{
    TreeNode *data;
    struct StackNode *next;
}StackNode;

/**
 * @brief 二叉树的创建，注意：用二级指针原因是修改了一级指针的内容
 *        根左右的创建方式
 * @param T 二叉树
 * @param data 用于建立二叉树的字符串
 * @param index 用于在递归中记录当前第N个字符
 */
void CreateTree(TreeNode **T, char *data, int *index)
{
    char ch;
    ch = data[*index];
    *index += 1;
    if(ch == '#')
    {
        *T = NULL; //若为#则为空节点
    }
    else
    {
        *T = (TreeNode*)malloc(sizeof(TreeNode));
        (*T)->data = ch;
        (*T)->flag = 0; 
        //根据递归思路知道：它会先创建左边的遇到#然后递归回来在创建右边的
        CreateTree(&((*T)->lchild), data, index); //创建左子树
        CreateTree(&((*T)->rchild), data, index); //创建右子树
    }
}

/**
 * @brief 栈的初始化
 * 
 * @return StackNode* 
 */
StackNode* InitStack()
{
    StackNode *s = (StackNode*)malloc(sizeof(StackNode));
    s->data = NULL;
    s->next = NULL;
    return s;
}
/**
 * @brief 入栈操作
 * 
 * @param data 
 * @param s 
 */
void Push(TreeNode *data, StackNode *s)
{
    StackNode *node = (StackNode*)malloc(sizeof(StackNode));
    node->data = data;
    node->next = s->next;
    s->next = node;
}
/**
 * @brief 判断栈空
 * 
 * @param s 
 * @return int 1：空 0：非空
 */
int IsEmpty(StackNode *s)
{
    if(s->next == NULL)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
/**
 * @brief 出栈操作
 * 
 * @param s 
 * @return StackNode* 
 */
StackNode *Pop(StackNode *s)
{
    if(IsEmpty(s))
    {
        return NULL;
    }
    else
    {
        StackNode *node = s->next;
        s->next = node->next;
        return node;
    }
}
/**
 * @brief 非递归先序遍历，下面代码给出详细介绍
 * 
 * @param T 
 */
void PreOrder(TreeNode *T)
{
    TreeNode *node = T;
    StackNode *s = InitStack(); //建栈对树木的节点压栈
    while(node || !IsEmpty(s)) //如果节点非空或者栈非空
    {
        /*
        例如例子 AB##C##先输出了A节点，然后把A节点压入栈中 node指向A的左孩子，进入下一轮循环，继续走if选项，输出之后再次压入栈中,node指向B节点的左孩子，
        但是B节点的左孩子为空，故在下一轮循环中B节点将从栈中弹出，用node接收，访问它的右孩子，此时右孩子也为空，则再次返回，然后将A节点弹出，node接收它
        的右节点也就是C节点，然后输出C节点，再将其压入栈中，发现它的左孩子为空，故将其从栈中弹出，然后访问它右孩子，发现也是空。此时栈中为空且node为空
        故结束本次遍历输出ABC
        */
        if(node)
        {
            printf("%c ",node->data);
            Push(node, s);
            node = node->lchild;
        }
        else
        {
            node = Pop(s)->data;
            node = node->rchild;
        }
    }
}
/**
 * @brief 非递归中序遍历
 * 
 * @param T 
 */
void InOrder(TreeNode *T)
{
    TreeNode *node = T;
    StackNode *s = InitStack();
    while(node || !IsEmpty(s))
    {
        /*
        同样拿上面先序遍历例子来说：首先是A节点，先压入栈中，找它左孩子，其左孩子为B节点，B节点在压入栈中，因B节点左孩子为空，则弹出B节点，然后输出B节点的值，
        在看它的右孩子，发现它的右孩子也为空，所以弹出A节点并输出它的值，然后查看它的右孩子，右孩子是C节点不为空，压入栈中，在访问左孩子，但为空，然后弹出栈
        并且直接访问它，然后查看它的右孩子，为空，然后栈空，node为NULL故循环结束，则输出BAC
        */
        if(node)
        {
            Push(node,s);
            node = node->lchild;
        }
        else
        {
            node = Pop(s)->data;
            printf("%c ",node->data);
            node = node->rchild;
        }
    }
}
/**
 * @brief 获取栈顶（并非弹出栈顶，只是获取），用于非递归后序遍历中使用
 * 
 * @param s 
 * @return StackNode* 
 */
StackNode* GetTop(StackNode *s)
{
    if(IsEmpty(s))
    {
        return NULL;
    }
    else
    {
        StackNode *node = s->next;
        return node;
    }
}

/**
 * @brief 非递归后续遍历
 * 
 * @param T 
 */
void PostOrder(TreeNode *T)
{
    TreeNode *node = T;
    StackNode *s = InitStack();
    /*
    不同于先序遍历和中序遍历，后续遍历更为复杂。
    举同上面例子来说明，首先A节点被压入栈中，访问它的左孩子也就是B节点，但B左孩子为空即node为空故走else,发现B节点右孩子为空故继续走嵌套那个else,
    此时B节点弹出用top接收，将其输出，然后B节点的flag标志置1，需要注意的是node还是空的并没有改变，于是继续走第一个else,获取栈顶是A节点,用top接收，
    发现其右孩子C节点不为空，并且它的flag为0，于是将其右孩子C节点压入栈中。node节点指向C节点的左孩子，发现它的左孩子为空,故下个循环先走第一个else，
    然后发现它的右也为空，于是继续走第二个else，弹出栈顶C节点并且输出，另令其标志位置1，注意node此次循环并未改变，于是走下一轮循环时候因node为空走else，
    获取它栈顶节点A，它的右孩子不为空但是它的右孩子标志位已经置1，于是走第二个else弹出A节点，输出，然后令A节点标志位置1，此时node还是为空，且栈为空故
    结束循环
    又如：ABC##D#E##F##
    例子流程：A入栈-->B入栈-->C入栈-->C左孩子为空走else，栈顶为C，右孩子为空接着走else，弹出C节点输出C,且标志位置1-->
    node还是为空继续走else,获取栈顶B，有右孩子D节点且标志位为0，将D压入栈中,node找它的左孩子为空-->node空走else，获取栈顶D节点，D节点
    有右孩子E节点且标志位为0，将E节点压入栈中,E节点左孩子为空，即node为空-->node空走else，获取栈顶E节点，为空不符合，再走else弹出栈顶E节点
    输出且另其标志位置1-->node没变化还是空，继续走else,获取栈顶节点D，其有右孩子且右孩子标志位为1，故走else节点D弹出栈输出且将其标志位置1，
    -->node依旧没变还是空，走else获取栈顶B节点，因其右孩子E标志已经置1，故走弹出B节点并输出置1-->node依旧没变，故这次循环依旧获取栈顶A节点，
    有右孩子（F节点）且没遍历过（flag==0），故将F节点压入栈中,node指向F的左孩子，但为空-->因node为空，这次循环依旧获取栈顶F节点，没孩子故走
    else弹出节点F，输出然后置1-->node依旧为空这次循环走else获取栈顶A节点，有右孩子但遍历过，故直接弹出A节点输出然后置为1

    */
    while(node || !IsEmpty(s))
    {
        if(node) //一样找到最左的树节点
        {
            Push(node, s);
            node = node->lchild;
        }
        else
        {
            TreeNode *top = GetTop(s)->data; //获取栈顶
            if(top->rchild && top->rchild->flag == 0)
            {
                top = top->rchild;
                Push(top,s);
                node = top->lchild;
            }
            else
            {
                top = Pop(s)->data;
                printf("%c ",top->data);
                top->flag = 1;
            }
        }
    }
}

int main(int argc, char *argv[])
{
    TreeNode *T;
    int index = 0;
    //修改一级指针传其地址上述用二级指针接收，传第一个命令行参数的字符串
    // CreateTree(&T, argv[1], &index); 
    // char *s = "AB##C##"; //根A 左B 右C
    char *s = "ABC##D#E##F##";
    CreateTree(&T, s, &index);
    PreOrder(T);
    printf("\n");
    InOrder(T);
    printf("\n");
    PostOrder(T);
    printf("\n");
    return 0;
}
```



## 线索二叉树

*前驱节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的前一个节点为该节点的前驱节点；*

*后继节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的后一个节点为该节点的后继节点；*

*例如一颗完全二叉树（1,2,3,4,5,6,7），按照中序遍历后的顺序为：（4,2,5,1,6,3,7），1节点的前驱节点为：5，后继节点为6.*

[参考链接](https://blog.csdn.net/hpcds/article/details/88706413 )

*如果节点有左孩子，那么Lchild依然指向他的左孩子，否则指向遍历序列中他的前驱节点。*

*如果节点有右孩子，那么Rchild依然指向他的左孩子，否则指向遍历序列中他的后继节点。*

*Ltag和Rtag的定义如下:*

*Ltag : 等于0时，Lchild域指示节点的左孩子;等于1时，Lchild指示节点的遍历前驱。*

*Rtag : 等于0时，Rchild域指示节点的左孩子;等于1时，Rchild指示节点的遍历后继。*

[参考链接](https://blog.csdn.net/kcycxy/article/details/109729998 )

### 中序线索二叉树

```C
#include<stdio.h>
#include<stdlib.h>

typedef struct TreeNode
{
  char data;
  struct TreeNode *lchild;
  struct TreeNode *rchild;
  int ltag;
  int rtag;
}TreeNode;

/**
 * @brief 创建二叉树
 * 
 * @param T 
 * @param data 
 * @param idnex 
 */
void CreateTree(TreeNode **T, char *data, int *index)
{
  char ch;
  ch =data[*index];
  *index += 1;
  if(ch == '#')
  {
    *T = NULL; //#代表空节点
  }
  else
  {
    *T = (TreeNode*)malloc(sizeof(TreeNode));
    (*T)->data = ch;
    (*T)->ltag = 0;
    (*T)->rtag = 0;
    CreateTree(&((*T)->lchild), data, index); //创建左子树
    CreateTree(&((*T)->rchild), data, index); //创建右子树
  }
}

/**
 * @brief 中序遍历：简言之：先处理左孩子->办事->处理右孩子 左->中->右
 *
 * @param T
 */
void InOrder(TreeNode* T)
{
    if (T == NULL)
    {
        return;
    }
    else
    {
        InOrder(T->lchild); //打印左孩子
        printf("%c ", T->data);
        InOrder(T->rchild); //最后打印右孩子
    }
}

/**
 * @brief 中序线索二叉树 
*        *pre一直指向T之前指向的那个，前驱的话就是T->left=*pre，后继是*p->right = T
 * @param T 
 * @param pre 始终指向刚刚访问过的节点 也可以定义一个全局一级指针变量的pre
 */

void InThreadTree(TreeNode *T, TreeNode **pre)
{
  if(T)
  {
    InThreadTree(T->lchild, pre); //递归左子树线索化
    if(T->lchild == NULL) //没有左孩子
    {
      T->ltag = 1; //前驱线索
      T->lchild = *pre; //左孩子的指针指向前驱
    }
    if(*pre != NULL && (*pre)->rchild == NULL) //前驱不为空，且没有右孩子
    {
      (*pre)->rtag = 1; //后继线索
      (*pre)->rchild = T; //前驱的右孩子指向后继，当前节点T
    }
    *pre = T;
    InThreadTree(T->rchild, pre); //递归右子树的线索化
  }
}
/**
 * @brief 获取第一个值：线索化之后最左的那个节点（左孩子为空）肯定指向了它的前驱
 * 
 * @param T 
 * @return TreeNode* 
 */
TreeNode* GetFirst(TreeNode *T)
{
  while(T->ltag == 0)
  {
    T = T->lchild;
  }
  return T;
}
/**
 * @brief Get the Next object
 * 
 * @param node 
 * @return TreeNode* 
 */
TreeNode *GetNext(TreeNode *node)
{
  /*
  举例：AB#C##D## 中序遍历为BCAD且B节点左孩子为空，C节点左右孩子都空，D左右孩子都空 
  所以B节点左孩子指向它的前驱，没有即NULL，C的左指向B，C的右指向A   D的左指向A，D的右指向NULL
  线索化后，查找该节点的下一个即：找到右孩子指向了后继的节点
  如：假设找B节点的下一个：因为它的rtag == 0即还有右孩子，故找它的右孩子C节点作为根节点的最左的那个值，因为没有左孩子，故直接返回，也就是返回了C节点
  再找C的下一个节点，它的右指向了后继A，也就是rtag==1，故直接返回它的后继A节点， 再找A节点的下一个，有右孩子走else，找以D为根节点的树最左的那个，这里
  也就是它自己，故将D节点返回，再找D节点下一个发现为空NULL--也就是遍历完成了
  */
  if(node ->rtag == 1) 
  {
    return node->rchild;
  }
  else
  {
    return GetFirst(node->rchild);
  }
}

void InThreadTreePrint(TreeNode *T)
{
  for(TreeNode *node = GetFirst(T);node != NULL;node = GetNext(node))
  {
    printf("%c ",node->data);
  }
  printf("\n");
}

int main(int argc, char *argv[])
{
  TreeNode *T;
  TreeNode *pre = NULL;
  int index = 0;
  // CreateTree(&T, argv[1], &index);
  char *s = "AB#C##D##";
  CreateTree(&T, s, &index);
  InOrder(T);
  printf("\n");

  InThreadTree(T, &pre);
  printf("%c",pre->data);
  printf("\n");
  //pre已经指向最后一个节点，另其右孩子指向为空，这样Next函数遍历到它时候可直返回其右节点即NULL
  pre->rtag = 1;
  pre->rchild = NULL; 
  InThreadTreePrint(T);
  return 0;
}
```

### 先序线索二叉树

```C
/**/
```

### 后续线索二叉树

```C
/**/
```

