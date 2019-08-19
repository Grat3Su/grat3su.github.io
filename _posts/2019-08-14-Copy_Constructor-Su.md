---
title : "복사 생성자"
date: 2019-08-14
comment: false

---

### C++의 초기화

----

##### C의 초기화



```c++
int num = 20;
int &ref = num;
```



##### C++의 초기화

```c++
int num(20);
int &ref(num)
```



-> C++은 두 가지 초기화를 모두 지원



### 생성자/소멸자

---

- 객체의 초기화/마무리가 필요한 경우 유용하게 사용됨.
- 생성자
  - 객체 생성시 자동으로 호출되는 함수.
  - 자기 자신(클래스)와 같은 이름.
  - 자동으로 호출되기 때문에 return값이 없음.
- 소멸자
  - 메모리 해제 바로 전에 호출되는 함수
  - 마찬가지로 return 값을 지정할 수 없음.

- 객체 메모리 할당 -> 생성자 호출 -> ... ->소멸자 호출 -> 메모리 해제



### 동적할당시의 클래스 정의

---

- 클래스를 동적 할당 할 시 생성자/소멸자의 호출을 하지 않는다.
- 그래서 생성자/소멸자의 호출을 직접 작성해야 한다.

1. malloc을 사용하는 경우

   ```c++
   	NewClass *new4;
   	new4 = (NewClass*)malloc(sizeof(NewClass));
   
   	new4->NewClass::NewClass();
   	new4->ShowNum();
   	new4->~NewClass();
   
   	free(new4);
   ```

   - 프로그래머가 직접 생성자와 소멸자를 호출해야함

   

2. operator new를 사용하는 경우

   ```c++
   	NewClass *new3;
   
   	new3 = (NewClass*)operator new (sizeof(NewClass));
   	new3->NewClass::NewClass();
   	new3->ShowNum();
   	new3->~NewClass();
   
   	operator delete (new3);
   ```

   - operator new : new function
   - C++의 전역함수이기 때문에 헤더가 필요없음
   - 프로그래머가 직접 생성자와 소멸자를 호출해야함
   - 오버로딩 가능

   

3. new를 사용하는 경우

   ```c++
   	NewClass *new3;
   	new3 =new NewClass;
   	new3->ShowNum();
   
   	delete (new3);
   ```

   - new expression : 내부적으로 new function을 호출하고 있음.
   - 자동으로 생성자와 소멸자가 호출됨
   - 오버로딩 불가능



### 얕은 복사 ( = 디폴트 복사)

---

1. 자동으로 삽입이 된다.
2. ㅇㄴ





---

### 깊은 복사 ( = 사용자 지정 복사)

---

1. 오..음...
2. 으...



