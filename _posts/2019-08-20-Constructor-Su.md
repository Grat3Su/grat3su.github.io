---
title : "생성자"
date: 2019-08-20
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
  
  - 매개변수의 할당이 없는 경우 디폴트 생성자라고 한다.
  
  - 생성자를 오버로딩하여 적절한 생성자가 호출되도록 만들 수 있음.
  
- 소멸자
  - 메모리 해제 바로 전에 호출되는 함수
  - 마찬가지로 return 값을 지정할 수 없음.

- ```c++
    class NewClass
    {
    public:
    	NewClass();//디폴트 생성자
    	NewClass(int num1, float num2);//생성자 오버로딩
    	~NewClass();//소멸자
    
    	void ShowNum();
    
    private:
    	int i_num;
    	float f_num;
    };
    
void main()
    {
        NewClass new1;
    	NewClass new2(50, 16.5f);
        NewClass new3 = 
    }
    ```
    
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

   1.   일반 할당
   ```c++
   	NewClass *new3;
   	new3 =new NewClass;
   	new3->ShowNum();

   	delete new3;
   ```
   2. 배열 할당  
   
  ```c++
   NewClass *new5;
   	new5 = new NewClass[3];//디폴트 생성자로 호출
   	new5->ShowNum();
   	new5[0].ShowNum();
   	new5[1].ShowNum();
   	new5[2].ShowNum();
   	delete[] new5;
  ```

   - 배열의 경우, 각각의 생성자를 호출
   - new expression 
     - 내부적으로 new function을 호출하고 있음.
     - new operator로 메모리를 할당하고 그 타입에 대한 생성자 호출
   - 자동으로 생성자와 소멸자가 호출됨
   - 오버로딩 불가능



연산자 오버로딩에서는 new operator만 오버로딩 가능. 생성자 호출을 막을수 없다.



암시적 호출 : C++ 컴파일러는 함수의 인자로 들어오는 값을 알아서 타입에 맞게 바꿀 수 있다.

```c++
class AAA{
public:
	AAA();
	AAA(int i) : num(i);
	~AAA();
private:
	int num;
};

void A(AAA a)
{
    cout<<"작동함"<<endl;
}

void main()
{
    AAA a = 5;
   	A(3);
}
```

A는 AAA 클래스를 받아야하는데 마치 변수를 받는것 처럼 쓰이고 있다. 

main에서 A에 변수를 보내더라도  C++ 컴파일러가 AAA 클래스를 읽어오면서 적절한 형식을 찾아 대입하기 때문에 정상적으로 작동하게 된다.



```c++
class AAA{
public:
	AAA();
	explicit AAA(int i) : num(i);
	~AAA();
private:
	int num;
};

void A(AAA a)
{
    cout<<"작동함"<<endl;
}

void main()
{
    AAA a(5);
   	A(AAA(3));//AAA타입의 객체를 만드는 문장
    AAA temp = AAA(3);//위의 것과 동일한 작동 temporary object
}
```

생성자 앞에 explicit를 붙이면 명시적인 생성자 호출만 받게되어 main의 A는 작동하지 않는다.

