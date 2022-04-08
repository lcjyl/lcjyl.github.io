---
数据结构链表篇（01）
---
	/**************************************************************
	*	主要实现功能：头插法、尾插法、查找
	*	日期：二零二一年十一月十一日
	*			不夜行者
	***************************************************************/
	
	#include<stdio.h>
	#include<stdlib.h>
	
	typedef struct Node
	{
		int data;     //数据域
		struct Node* next; //指针域
	}Node;
	
	//创建链表（表头）
	Node* Create()
	{
		Node* L = (Node*)malloc(sizeof(Node));
			L->next = NULL; //初始化为NULL
			L->data = 0; //开始节点数为0（不包括头结点）
			return L;
	}
	
	
	/*	
	*	头插法（头结点的后一个位置）
	*	参数：L需要插入数据的链表  data需要插入的数据
	*/
	void HeadInser(Node* L, int data)
	{
			Node* Y = (Node*)malloc(sizeof(Node));
			Y->data = data;
			Y->next = L->next;//新的节点指向头结点指向的那一个节点
			L->next = Y;  //头结点指向新的节点
			L->data++;//头结点数增加
	}
	
	
	/*	
	*	尾部插法
	*	参数：L需要插入数据的链表  data需要插入的数据
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
	/*
	* 打印传入的L链表
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
	
	/*
	* 查找L链表中data值  若找到则打印该值 否则打印不存在该值
	*/
	void List_Find(Node* L,int data)
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
	
	/*
	* 删除链表L中第一个值为data的节点
	*/
	
	void List_Delet(Node* L, int data)
	{
		Node* Y = L;
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
					printf("删除值为%d后的链表: ",P->data);
					free(P);
					P = NULL;
					return;
				}
				Y = Y->next;
			}
		}
		L->data--;
		printf("没有找到需要删除的结点\n");
	}
	//链表销毁不包括头结点（空的）
	void List_Destroy(Node* L)
	{
		Node* Y = L;
		while (L)
		{
			L = L->next;
			free(Y);
			Y = NULL;
			Y = L;
		}
		if (L == NULL)
		{
			printf("删除链表成功");
		}
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
		List_Delet(LY, 6);
		PrintList(LY);
		List_Destroy(LY);
		return 0;
	}
	
