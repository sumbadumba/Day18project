#pragma once

#ifndef stdafx_h
#define stdafx_h

#include<iostream>
#include<string>
#include<stdlib.h>
#include<time.h>
#include<stdio.h>
#include<vector>
#include<Windows.h>


using namespace std;

#endif


#pragma once
#include"stdafx.h"

template<typename T>

class TClass
{
public:
	TClass(string name, T age);


	~TClass();
	

	void Function()
	{
		cout << age << endl;
	}

private:
	T age;//T의 인스턴스가 생기기 전까지는 무슨 값이 있는 지는 모른다.

	string name;

};

// 템플릿 함수들을 헤더에 정의하는 이유
// 템플릿이 사용되는 부분에서 해당 템플릿을 객체로 만들려면 
// 그 템플릿이 어떻게 생겼는지 컴파일러가 알아야 하기 때문이다.

template<typename T>
inline TClass<T>::TClass(string name, T age)
	:name(name),age(age)
{

}


template<typename T>
inline TClass<T>::~TClass()
{

}




#include"stdafx.h"
#include"TClass.h"


//template<typename T>
// T(형 반환가능) 함수 원형(T t)
//{
//	함수 내용
//	T data;
//}
//함수 template의 T는 지역성을 갖고있다고 말할 수 있다.
//선언을 한번 더 할때 새로 생성을 해야 함.
template<typename T>
T Function(int value)//인트형 value를 반환(T형으로)(캐스팅을 해줘야함)casting(할당)
{
	return value;
}

template<typename T>
T ADD(T value, T value2)
{
	cout << "함수 원형 " << endl;

	return value + value2;
}

template<>
double ADD<double>(double value, double value2)//많이 쓰지는 않는데 특수처리를 하고싶을때 쓴다.
{
	cout << "명시적 특수화(double) " << endl;

	return value + value2;
}

template<typename T,typename U>
T Subtract(T value, U value2)
{
	cout << "함수 원형 (자료형 두개) " << endl; 

	return value - value2;
}
template<typename T, typename = typename enable_if <//enable은 활성화
	is_same<T, int>::value ||
	is_same<T, float>::value ||
	is_same<T, double>::value||
	is_same<T, unsigned int>::value
>::type>

T Multiply(T value)
{
	return value * 2;

}

int main()
{
	int i = Function<float>(10.0f);
	float f = Function<float>(10.0f);

	//printf("%d\n",i);//이것처럼 int형으로 제대로 캐스팅을 해줘야 ㅎ나다.

	//printf("%d\n", f);

	//printf("%f\n", f);

	//printf("%d\n", Function<float>(10.0f));

	//printf("%f\n", Function<float>(10.0f));

	//ADD<int>(10, 20);//int형으로 받아야함.
	//ADD<float>(10.0f, 20.0f);//float형으로 받아야함.
	//ADD<double>(10.0, 20.0);//double형으로 받아야함.
	////결론 : 그에 맞는 형을 꼭 써줘야함.


	//Subtract<int, unsigned int>(10, 20U);

	//Multiply(20);

	//Multiply(20.0);

	//short s = 5;

	//Multiply(s);//short형으로는 못 받아들임(INT형으로는 받음)

	TClass<int>obj("싹쓰리", 120);//인자값은 정의가 되있지 않아서 <int>, <float>로 정의를 해줘야 한다.

	obj.Function();

	return 0;
}

#숙제
1: Day21 숙제로 만든 링크드 리스트 클래스에 remove 기능추가하기
2: 어떤 타입의 데이터도 저장할 수 있도록 template를 사용한 링크드 리스트 클래스를 만들자




#pragma once

#ifndef stdafx_h
#define stdafx_h

#include<iostream>
#include<string>
#include<stdlib.h>
#include<time.h>
#include<stdio.h>
#include<vector>
#include<Windows.h>
#include<assert.h>


using namespace std;

#endif


#pragma once
#include"stdafx.h"
// Array_Stack

#define MAX_STACK_COUNT 5

//val을 이용한 stack을 구현하는 법

template<typename T>
class Array_Stack
{
public:
	Array_Stack()
	{
		//memset(values, 0, sizeof(T) * MAX_STACK_COUNT);//배열을 한꺼번에 초기화를 할 수 있다.
		ZeroMemory(values, sizeof(T) * MAX_STACK_COUNT);//배열의 값을 한꺼번에 0으로 clear해준다.


	}
	//집어 넣을때 사용
	void Push(T val)
	{
		//top+1dl max_stack_count 보다 작은지 물어봄..(스택은 쌓아올리는데 들어오는 최댓 값보다 하나가 작아야????한칸을 집어 넣을 수 있음
		//top이 맥스보다 작아야한다.
		//참일때는 아무 문제가 없는데 false일때는 값이 뻑난다.
		assert(top + 1 < MAX_STACK_COUNT);//assert주장하다
		values[++top] = val;//배열이 -부터 시작해서 선행증가를 사용한다.

	}

	//맨 위의 값을 가져올 때 사용
	T Top()//둘다 T를 return한다.
	{
		assert(Empty() == false);//비어있냐? 아니요(결론: 비어있지 않아야 문제가 없다는 것.)
		return values[top];
	}

	//꺼낼때 사용
	T Pop()
	{
		// 비어있으면 못 꺼낸다.
		assert(Empty() == false);

		T val = values[top];
		
		memset(&values[top--], 0, sizeof(T));


		return val;
	}

	//값이 비어있는지 검사할 때 사용
	bool Empty()
	{
		//bool result = top < 0 ? true : false;(이렇게 쓰일 수 있다)
		//뭐 ? 첫번째 : 두번째 (뭐?는 조건식이다 조건식이 true이면 첫번째것을 return)
		return top < 0 ? true : false;
	}


private:
	int top = -1;
	T values[MAX_STACK_COUNT];


};


#include"stdafx.h"
#include"Array_Stack.h"




int main()
{
	Array_Stack<int> s2;
	s2.Push(10);
	s2.Push(20);
	s2.Push(30);
	s2.Push(40);
	s2.Push(50);
	
	cout << s2.Pop() << endl;
	cout << s2.Top() << endl;//왜?40인가
	//pop에서 하나를 빼가지고 현재 제일 높은 40을
	//가져온다.

	return 0;
}


