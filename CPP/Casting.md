### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 캐스팅(형변환, Casting)
- #### Casting
    - 암시적(Implicit) 캐스팅
        - 컴파일러가 형을 변환해 줌
    - 명시적(Explicit) 캐스팅
        - 프로그래머가 형 변환을 위한 코드를 직접 작성

- #### C스타일 캐스팅
    - C++ 스타일 4개의 캐스팅 중 하나를 한다.
    - 뭔가 명확하지 못하다
    - 그래서 명백한 실수를 컴파일러가 캐치하지 못한다.
    - C++ 캐스팅이 이런 문제를 해결한다.
        - 좀 더 안전하게 컴파일도중에 프로그래머의 실수를 잡아 줄 수 있다.

- #### C++ 명시적 캐스팅
    - **static_cast**
        - 컴파일 도중에 결정이 된다. 즉, 컴파일 시에만 형 체크한다.
        ```C++
        // 값
        int number2 = static_cast<int>(number1);    // float number1

        // 개체 포인터
        Cat* myCat = static_cast<Cat*>(myPet);     // Animal* myPet;
        ```
    - **reinterpret_cast**
        - 연관 없는 두 포인터 형 사이의 변환을 허용
            - Cat*   <->   House*
            - Char*  <->   int*
        - 포인터와 포인터 아닌 변수 사이의 형 변환을 허용
            - Cat*   <->   unsigned int
        - 이진수 표기는 달라지지 않음
            - A형의 이진수 표기를 그냥 B형인 것처럼 해석
            ```c++
            int* signedNumber = new int(-10);
            unsigned int* unsignedNumber = reinterpret_cast<unsigned int*>(signedNumber);

            // unsignedNumber는 마이너스를 인지 하지 못하기 때문에
            // 바이너리 패턴을 보고 자기 마음대로 unsigend int인 것처럼 해석한다.

            // 결국 signedNumber과 unsignedNumber 둘다 -10을 가리키게 된다.
            // 위험하다!!
            ```
    - **const_cast**
        - const 지워주는 캐스팅이다. 즉, const_cast로 형을 바꿀 수 없음
        - const 또는 volatile 애트리뷰트를 제거할 때 const_cast를 사용한다.
        - const_cast를 사용할 때는 써드파티 라이브러리가 const를 제대로 사용하지 않을 때
            ```c++
            void WriteLine(char* ptr);              // const 없음! 뭔가 별로인 외부 라이브러리

            void MyWriteLine(const char* ptr)        // const 붙여서 받음. 우리 프로그램에 있는 함수
            {
                WriteLine(const_cast<char*>(ptr));   // const 제거해서 넘김
            }

            // const Animal* petPtr;
            Animal* animal = const_cast<Animal*>(petPtr);   // const 제거
            ```
    - **dynamic_cast** (C++98, 모던C++)
        - 실행 중에 형을 판단
        - 포인터 똔느 참조형을 캐스팅할 때만 사용할 수 있다.
        - 호환되지 않는 자식형으로 캐스팅하려 하면 NULL을 반환
        - 꽤 괜찮아 보이지만
        - 이걸 쓰려면 컴파일 중에 RTTI(실시간 타입정보, Real-Time Type Information)를 켜야 한다.
        - C++ 프로젝트에서 RTTI를 끄는 것이 보통

- #### 캐스팅 규칙
    - 제일 안전한 것 -> 가장 위험한 것
    - 기본적으로 **static_cast**를 쓸 것
    - reinterpret_cast<Cat*> 대신 **static_cast<Cat*>** 사용
        - 왜냐하면 만약 Cat이 Animal이 아니라면 컴파일러가 에러를 뱉음
    - 포인터와 비포인터 사이의 변환
        - 이걸 정말 해야 할 때가 있음
        - **reinterpret_cast**를 쓸 것
    - 내가 변경권한이 없는 외부 라이브러리를 호출할 때만 **const_cast**를 쓸 것