---
title: "Funcion Pointer"
date: 2019-04-26
comment: false
---



## 함수 포인터

#### 함수 주소 저장 변수

포인터의 장점 : 다른 변수를 참조하여 읽고 쓰는 것이 가능하다는 것이다.

하나의 함수 이름으로 필요에 다라 여러 함수를 사용하면 편리하다.

그것을 가능하게 해주는 것이 `함수 포인터`다.

함수 포인터는 함수의 주솟값을 저장하는 포인터 변수.

즉, 함수를 가리키는 포인터.

- 함수형 포인터는 반환형, 인자목록의 수와 각각의 자료형이 일치하는 함수의 주소를 저장할 수 있는 변수이다.
- 함수 포인터를 선언하려면 함수원형에서 함수이름을 제외한 반환형과 인자목록의 정보가 필요하다.





함수포인터를 이용한 함수 호출



함수 포인터 배열

포인터 배열과 같이 함수 포인터가 배열의 원소로 여러 개의 함수 포인터를 선언하는 함수 포인터 배열을 생각할 수 있다.

즉, 함수 포인터 배열은 함수 포인터가 원소인 배열이다.



void 포인터

- 포인터는 주솟값을 저장하는 변수인 int* / double* 같은 대상의 구체적인 자료형의 포인터로 사용하는 것이 일반적이다
- 주솟값이란 참조를 시작하는 주소에 불과하며, 자료형을 알아야 참조할 범위와 내용을 해석할 방법을 알 수 있는 것이다.
- void*는 자료형을 무시하고 주솟값만을 다루는 포인터이다.
- 그러므로 void포인터는 대상에 상관없이 모든 자료형의 주소를 저장할 수 있는 만능 포인터로 사용할 수 있다.
- void 포인터는 모든 주소를 저장할 수 있지만 가리키는 변수를 참조하거나 수정이 불가능하다.
- 주솟값으로 변수를 참조하려면 결국 자료형으로 참조범위를 알아야하는데 void 포인터는 이러한 정보가 전혀 없이 주소만을 담는 변수에 불과하기 때문이다.
- 그러므로 실제 void 포인터로 변수를 참조하기 위해서는 자료형 변환이 필요하다.



![image](https://user-images.githubusercontent.com/26815767/56794006-3d7c4880-6848-11e9-893c-a37f44015fca.png)



![image](https://user-images.githubusercontent.com/26815767/56794044-4e2cbe80-6848-11e9-8f6d-9f2cf59061a3.png)