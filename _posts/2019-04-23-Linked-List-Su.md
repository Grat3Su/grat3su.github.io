---
title: "Linked List"
date: 2019-04-16
comment: false
---

### 링크드 리스트

1. 고정 길이인 배열에 비해 길이가 가변적이다. 
2. 중간에 데이터 삽입, 삭제가 용이하다.



#### 링크드 리스트의 종류

- 싱글 링크드 리스트
  - 다음 노드로만 연결
- 더블 링크드 리스트
  - 다음 노드와 전 노드를 연결

![image](https://user-images.githubusercontent.com/26815767/56560218-c42ded00-65de-11e9-8ecf-0f30cf0a8b6e.png)





#### Insert



![image](https://user-images.githubusercontent.com/26815767/56560238-d0b24580-65de-11e9-8a0b-e11728bc3808.png)



```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
#include<string.h>

struct People
{
	int age;
	char name[10];
	People* next;//포인터로 자기참조 가능
};

People* headnode = NULL;

People* createPeople()
{
	People* p = new People;
	p->next = NULL;
	return p;
}

void DeletePeople(People* p)
{
	People* current = p;
	while (current != NULL)
	{
		People* next =  current->next;
		delete current;
		current = next;
	}
}
void SetPeople(People* p, int age_, const char* name_)
{
	p->age = age_;
	strcpy(p->name, name_);
}

void Insert(People* curnode, People* newnode)
{
	People* prevnode = curnode->next;
	curnode->next = newnode;
	newnode->next = prevnode;
}
void PrintPeople(People* p)
{
	People* current = p;
	while (current !=NULL)
	{
		printf("%s : %i\n", current->name, current->age);
		current = current->next;
	}
	
}

int main()
{
	headnode = createPeople();
	SetPeople(headnode, 1, "hello");

	People* p = createPeople();
	SetPeople(p, 2, "Hello");
	Insert(headnode, p);

	p = createPeople();
	SetPeople(p, 3, "World");
	Insert(headnode, p);
	

	PrintPeople(headnode);

	DeletePeople(headnode);
	

	_getch();
	return 0;
}
```

