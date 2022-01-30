### 출력(Output)
- namespace
    - 함수 이름, 클래스 이름, 기타 등등 이름 충돌을 피하기 위하기 위해
- using 지시문
    - 타이핑의 양을 줄이는 방법일 뿐임
- std::cout
- `<<` 연산자
- `#include <iomanip>` 안에 있는 조정자
    - setw()
    - setfill()
    - setprecision()
#
#### PrintMenu.h
```C++
#pragma once

namespace samples
{
	void PrintMenuExample();
}
```
#
#### PrintMenu.cpp
```C++
#include <iomanip>
#include <iostream>
#include "PrintMenu.h"

using namespace std;

namespace samples
{
	void PrintMenuExample()
	{
		cout << "+--------------------------+" << endl;
		cout << "|    Print Menu Example    |" << endl;
		cout << "+--------------------------+" << endl;

		const float coffeePrice = 1.25f;
		const float lattePrice = 4.75f;
		const float breakfastComboPrice = 12.104f;

		const size_t nameColumnLength = 20;
		const size_t priceColumnLength = 10;

		cout << left << fixed << showpoint << setprecision(2);

		cout << setfill('-')
			<< setw(nameColumnLength + priceColumnLength)
			<< ""
			<< endl 
			<< setfill(' ');

		cout << setw(nameColumnLength)
			<< "Name"
			<< setw(priceColumnLength) 
			<< "Price" 
			<< endl;

		cout << setfill('-')
			<< setw(nameColumnLength + priceColumnLength)
			<< ""
			<< endl
			<< setfill(' ');

		cout << setw(nameColumnLength)
			<< "Coffee"
			<< "$"
			<< coffeePrice
			<< endl;

		cout << setw(nameColumnLength)
			<< "Latte"
			<< "$"
			<< lattePrice
			<< endl;

		cout << setw(nameColumnLength)
			<< "Breakfast Combo"
			<< "$"
			<< breakfastComboPrice
			<< endl;
	}
}
```