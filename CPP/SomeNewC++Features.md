### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 일부 새로운 C++ 기능
- #### bool 데이터형
    ```C++
    // 만약 student가 아니면
    if (IsStudent() == false)
    {
        // ...
    }

    // 만약 student라면
    if (IsStudent() == true)
    {
        // ...
    }
    ```

- #### 참조자(reference)
    - 정의
        ```C++
        int number1 = 100;
        int number2 = 200;
        int& reference = number1;  // reference는 number1의 별칭된다.
        int& reference = number2;  // number1, reference, number2, 모두 값이 200이 된다.
                                   // 즉, reference 값의 변경은 number1의 값 변경으로 이루어진다.

        // errors
        int& reference = NULL;   // NULL이 될 수 없다.
        int& reference;          // 초기화 중에 반드시 별칭이 선언되어야 한다.
        ```

    - 함수 매개변수로서의 `포인터`
        ```C++
        void Swap(int* number1, int* number2)
        {
            int temp = *number1;
            *number1 = *number2;
            *number2 = temp;
        }
        ```
    - 함수 매개변수로서의 `참조`
        ```C++
        void Swap(int& number1, int& number2)
        {
            int temp = number1;
            number1 = number2;
            number2 = temp;
        }
        ```
    - 코딩표준
        - `읽기전용` 매개변수는 `상수 참조`로
        - `출력결과` 매개변수는 `포인터`로
            - TryDivide(&a, b, c);
            - 변수 a는 NULL이 될 수 있으니 : TryDivide(NULL, b, c);
            - 함수 내에 assert함수를 넣어 a가 NULL이 되는 경우를 잡아낸다.
        ```C++
        bool TryDivide(Vector* result, const Vector& b, const Vector& c);

        TryDivide(&a, b, c);
        ```

- #### nullptr
    - NULL
        ```c++
        #define NULL 0     // 숫자

        int number = NULL  // ok
        int* ptr = NULL    // ok
        ```
    - nullptr
        ```c++
        typedef decltype(nullptr) nullptr_t;  // null포인터 상수

        int anotherNumber = nullptr;   // error
        int* anotherPtr = nulptr;      // ok
        ```
        ```c++
        // Main.cpp
        Class* myClass = new Class("COMP3200");

        const Student* student = myClass->GetStudent("Coco");
        if (student != nullptr)
        {
            std::cout << student->GetID() << ":" << student->GetName() << std::endl;
        }
        ```
    - 포인터에는 언제나 nullptr를 쓰자!
    - 더 이상 NULL이 있을 곳이 없음!!!!

- #### 고정 폭 정수형(fixed-width integer types)
    - C++11에서 지원됨
        - int8_t  / uint8_t
        - int16_t / uint16_t
        - int32_t / uint32_t
        - int64_t / uint64_t
        - intptr_t / uintptr_t
        - 그리고 더 많이 있음: http://en.cppreference.com/w/cpp/types/integer
    - 가독성 향상을 위해 낡은 기존 자료형보다 이것들 쓰자
        - int8_t score = student->GetScore();

- #### enum class
    - C++11에서 제대로 지원
    - 정수형으로의 암시적 캐스팅이 없음
    - 자료형 검사!
    - 또한 enum에 할당할 바이트 양을 정할 수도 있음
    - C스타일 Enum 문제점
        ```c++
        // Main.cpp
        enum eScoreType
        {
            Assignment1,   // = 0
            Assignment2,
            Assignment3,
            Midterm,
            Cout,         // = 4
        };

        enum eStudyType
        {
            Fulltime,    // = 0
            Parttime,    // = 1
        };

        int main()
        {
            eScoreType type = Midterm;
            eStudyType studyType = Fulltime;

            int num = Assignment3;  // 에러 안나고 넘어감. 이건 큰 문제!

            if (type == Fultime)    // 에러 안나고 넘어감. 이건 큰 문제!
            {
                // ...
            }
            
            return 0;
        }
        ```
    - 해결책: enum class
        ```c++
        // Main.cpp
        enum class eScoreType
        {
            Assignment1,   
            Assignment2,
            Assignment3,
            Midterm,
            Cout,         
        };

        enum eStudyType
        {
            Fulltime,    
            Parttime,    
        };

        int main()
        {
            eScoreType score = eScoreType::Midterm;
            eStudyType studyType = eStudyType::Fulltime;

            int num = eScoreType::Assignment3;  // 에러가 난다.

            if (score == eScoreType::Fultime)    // 에러가 난다.
            {
                // ...
            }
            
            return 0;
        }
        ```
    - enum class용 정수형 명시하기
        ```c++
        // Main.cpp
        enum class eScoreType : uint8_t   // 타입 명시
        {
            Assignment1,   
            Assignment2,
            Assignment3,
            Midterm,
            Final = 0x100,  // 경고 뜬다.         
        };
        ```