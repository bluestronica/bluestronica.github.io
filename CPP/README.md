### [bluestronica.github.io](https://bluestronica.github.io/)

### [고찰](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Discussion.md)
- (char*)&vector

### [비트플래그(Bitflag)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Bitflag.md)
- 비트 플래그 정의
- 비트 켜기
- 비트 끄기
- 비트 토글
- 비트 상태 확인

### [출력(Output)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Output.md)
- namespace
- using 지시문
- std::cout
- `<<` 연산자
- `#include <iomanip>` 안에 있는 조정자 

### [입력(Input)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Input.md)
- std::cin
- 키보드에서 안전하게 읽기

### [일부 새로운 C++ 기능](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/SomeNewC++Features.md)
- bool 데이터형
- 참조자(reference)
- nullptr
- 고정 폭 정수형(fixed-width integer types)
- enum class

### [문자열(string)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/string.md)
- std::string 클래스
- `<sstream>`
- 여전히 성능상의 이유로 많은 C함수들을 사용

### [파일 입출력(I/O)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/File_IO.md)
- `<fstream>`
- 파일 읽기
- 파일에 쓰기
- 바이너리 파일 읽기
- 바이너리 파일에 쓰기
- 파일 안에서 탐색
- 탐색(seek) 유형
- 기타 정보

### [개체지향 프로그래밍 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/OOP_1.md)
- OOP C++
- 보통 접근 제어자(Access Modifier) 별로 C++ 멤버들을 그룹 지음
- 개체 생성
- 개체 배열(Array)
- 개체 소멸(delete)
- 초기화와 대입의 차이를 이해한다.
- 기본 생성자
- 생성자 오버로딩(Overloading)
- 소멸자(Destructor)
- 클래스 멤버 함수
- Const 멤버 함수
- 구조체에 관한 코딩표준

### [개체지향 프로그래밍 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/OOP_2.md)
- 복사 생성자
- 클래스 안에서 동적으로 메모리를 할당하고 있다면 직접 메모리 해제를 하는 코드 작성해야 한다.
- 함수(메서드) 오버로딩
- 연산자
- 멤버 함수를 이용한 연산자 오버로딩
- friend 키워드
- 멤버 아닌 함수를 이용한 연산자 오버로딩
- 연산자 오버로딩과 const, &
- 대입(assignment) 연산자
- 클래스에 딸려오는 기본 함수들 (컴파일러의 암시적 생성)
- 클래스에 딸려오는 기본 함수들 `지우는` 법

### [개체지향 프로그래밍 3](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/OOP_3.md)
- 상속
- 다형성과 관련있는 `멤버 함수의 메모리`에 대해 알아보자
- 정적 바인딩 - 멤버 함수
- 동적 바인딩 - 다형성의 핵심인 가상(virtual) 멤버 함수
- C++ 다형성의 이해
- 가상(virtual) 소멸자
- 순수 가상함수
- 추상 클래스
- 인터페이스

### [캐스팅(형변환, Casting)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Casting.md)
- 캐스팅(Casting)
- C스타일 캐스팅
- C++ 명시적 캐스팅
- 캐스팅 규칙

### [인라인 함수(Inline Function)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Inline_Function.md)
- 함수 호출할 때
- 인라인 함수의 동작 원리
- 인라인 함수를 쓸 때 주의점

### [static 키워드](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Static.md)
- static
- 정적(static) 변수
- 정적(static) 멤버 변수
- 정적(static) 멤버 변수 베스트 프랙티스
- 정적(static) 멤버 함수

### [예외(Exception)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Exception.md)
- C++에서의 예외(Exception)
- 웹 애플리케이션에서의 에러 처리
- 적절한 예외 처리 전략

### [표준 템플릿 라이브러리(STL, Standard Template Library) 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/STL1.md)
- STL 컨테이너 목록
- 벡터(vector)

### [표준 템플릿 라이브러리(STL, Standard Template Library) 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/STL2.md)
- 맵(map)
- 셋(set)
- unordered_map
- unordered_set
- array
- 범위 기반 for 반복문

### [표준 템플릿 라이브러리(STL, Standard Template Library) 3](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/STL3.md)
- 큐(queue)
- 스택(stack)
- 리스트(list)
- STL 컨테이너가 더 있다
- 회사내부에서 만드는 STL 대체품들...

### [템플릿(Template) 프로그래밍](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Template.md)
- 템플릿이란?
- 함수 템플릿
- 템플릿은 어떻게 작동할까?
- 선언과 구현체가 헤더파일 한 곳에 넣는다.
- 클래스 템플릿
- 템플릿 특수화
- 장점과 단점
- 템플릿 프로그래밍 베스트 프랙티스

### [STL 알고리듬(Algorithm)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/STL_Algorithm.md)
- STL 알고리듬이란?
- STL 알고리듬의 유형
- vector를 다른 vector로 복사하기
- STL 알고리듬 목록

### [새로운 키워드 (C++11 ~)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/NewKeywords11-17.md)
- auto
- static_assert
- default / delete
- final / override
- offsetof 매크로

### [스마트(Smart) 포인터 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/SmartPointer1.md)
- 원시 포인터의 문제점
- 유니크 포인터(unique_ptr)
- 유니크 포인터는 다음의 세 경우에 적합하다.
- 유니크 포인터 만들기 - std::make_unique()
- 유니크 포인터 재설정하기 - reset()
- 포인터 가져오기 - get()
- 포인터 소유권 박탈하기 - release()
- 포인터 소유권 옮기기 - std::move()
- std::unique_ptr
- std::unique_ptr 베스트 프랙티스

### [스마트(Smart) 포인터 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/SmartPointer2.md)
- 자동 메모리 관리
- 가비지 컬렉션
- 참조 카운팅
- 수동 참조 카운팅
- 강한(strong) 참조
- 참조 카운팅의 문제점
- 가비지 컬렉션 vs 참조 카운팅

### [스마트(Smart) 포인터 3](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/SmartPointer3.md)
- 공유 포인터(shared_ptr)
- 약한 포인터(weak_ptr)
- 정리

### [이동 생성자 및 이동 대입 연산자](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Move.md)
- 값(value)의 분류
- 과거 c++의 문제 (C++11 이전)
- rvalue 참조 (&&)
- std::move()
- 이동 생성자
- 이동 대입 연산자
- C++11 이후
- rvalue 최적화

### [constexpr](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Constexpr.md)
- constexpr의 의도
- 문자열 해쉬를 컴파일 도중에 만들 수 있다면?
- const vs constexpr
- const 변수 vs constexpr 변수
- cnost 함수 vs constexpr 함수
- 배열의 길이 정하기

### [람다 식(Lambda Expression)](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/LambdaExpression.md)
- 람다 식이란?
- 람다식의 장단점
- 베스트 프랙티스

### [가변 인자(Variadic) 템플릿](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Variadic.md)
- 가변 인자 템플릿
- 가변 인자 템플릿 함수
- 가변 인자 템플릿 활용

### [파일시스템(Filesystem), 모듈(Module)시스템](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/FilesystemModule.md)
- 파일시스템
- 파일시스템 연산
- 모듈 시스템

### [쓰레딩(Threading) 라이브러리](https://github.com/bluestronica/bluestronica.github.io/blob/main/CPP/Threading.md)
- 쓰레딩(Threading) 지원 라이브러리
- std::thread
- std::this_thread
- std::mutex
- std::scoped_lock
- std::condition_variable
- std::unique_lock