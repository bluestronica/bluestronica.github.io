### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 이동 생성자 및 이동 대입 연산자
- #### 값(value)의 분류
    - lvalue
        - 단일 식을 넘어 지속되는 개체
            - 주소가 있음
            - 이름이 있는 변수
            - const 변수
            - 배열 변수
            - 비트 필드(bit-fields)
            - 공용 구조체(unions)
            - 클래스 멤버
            - 좌측 값 참조(&)로 반환하는 함수 호출
            - 문자열 리터럴
        - 결국 지금까지 봐 온 많은 것들
    - rvalue
        - lvalue가 아닌 개체
        - 사용되는 단일 식을 넘어 지속되지 않는 일시적인 값
            - 주소가 없는 개체
            - 리터럴(무자열 리터럴 제외)
            - 참조로 반환하지 않는 함수 호출
            - i++와 i--
            - 기본으로 지원되는 산술실, 논리식, 그리고 비교식
            - 열거형(enum)
            - 람다(lambda)
    
- #### 과거 c++의 문제 (C++11 이전)
    ```c++
    // Math.cpp
    std::vector<float> Math::ConverToPercentage(const std::vector<float>& scores)
    {
        std::vector<float> percentages;
        for (std::vector<float>::cosnt_iterator it = scores.begin(); it != scores.end(); ++it)
        {
            // score를 percentage로 변환
        }
        return percentages;    
    }   // 복사 일어남

    // Main.cpp
    int main()
    {
        std::vector<float> scores;
        // ...
        scores = ConvertToPercentage(scores);  // 복사 일어남
        // ...

        // 복사하는 과정이 너무 많이 일어난다
        // 복사를 막을 방법은 rvalue 참조와 이동 문법으로 해결할 수 있다.
    }
    ```

- #### rvalue 참조 (&&)
    - C++11 이후에 새로 나온 연산자
    - 기능상 & 연산자와 비슷
    - & 연산자는 lvalue 참조에 사용
    - && 연산자는 rvalue 참조에 사용

- #### std::move()
    - rvalue 참조를 반환
    - lvalue를 rvalue로 반환

- #### 이동 생성자
    - 다른 개체 멤버 변수들의 소유권을 가져 옴
    - 복사 생성자와 달리, 메모리 재할당을 하지 않음
    - 복사 생성자보다 빠름
    - 약간 얕은 복사와 비슷
        ```c++
        MyString::MyString(MyString&& other) 
        {
            // ...
        }
        ```
    - 복사 생성자를 사용한 코드
        ```c++
        // MyString.h
        class MyString
        {
        public:
            MyString(const MyString& other);   // 복사 생성자
            MyString(const char* str);         // 매개변수를 가지는 생성자
            // ...
        private:
            char* mString;
            int mSize;
        }
        ```
        ```c++
        // MyString.cpp
        MyString::MyString(const MyString& other)    // 복사 생성자 구현
            : mSize(other.mSize)
        {
            mString = new char[mSize];
            memcpy(mString, other.mString, mSize);
        }

        MyString::MyString(const char* str)          // 매개변수를 가지는 생성자 구현
            : mSize(strlen(str) + 1)
        {
            mString = new char[mSize];
            memcpy(mString, str, mSize);
        }
        ```
        ```c++
        // Main.cpp
        MyString studentName("Lulu");      // 생성자
        MyString copiedName(studentName);  // 복사 생성자
        ```
    - 이동 생성자를 사용한 코드
        ```c++
        // MyString.h
        class MyString
        {
        public:
            MyString(MyString&& other);         // 이동 생성자
            MyString(const MyString&& other);   // 복사 생성자
            MyString(const char* str);          // 매개변수를 가지는 생성자
            // ...
        private:
            char* mString;
            int mSize;
        }
        ```
        ```c++
        // MyString.cpp
        MyString::MyString(MyString&& other)       // 이동 생성자 구현
            : mString(other.mString)
            , mSize(ohter.mSize)
        {
            other.mString = nullptr;
            other.mSize = 0;
        }

        MyString::MyString(const MyString& other)    // 복사 생성자 구현
            : mSize(other.mSize)
        {
            mString = new char[mSize];
            memcpy(mString, other.mString, mSize);
        }

        MyString::MyString(const char* str)          // 매개변수를 가지는 생성자 구현
            : mSize(strlen(str) + 1)
        {
            mString = new char[mSize];
            memcpy(mString, str, mSize);
        }
        ```
        ```c++
        // Main.cpp
        MyString studentName("Lulu");      // 생성자
        MyString copiedName(std::move(studentName));  // 이동 생성자

        // studentName을 임시 개체로 전달해줘서 소유권이 넘어간 형태다
        // 임시 개체가 메모리를 가지고 있으니깐
        // studentName은 텅텅 비워지게 된다.
        ```

- #### 이동 대입 연산자
    - 이동 생성자와 같은 개념
    - 다른 개체 멤버 변수들의 소유권을 가져 옴
    - 이것도 메모리 재할당을 하지 않음
    - 얕은 복사
        ```c++
        MyString& MyString::operator=(MyString&& other)
        {
            // ...
        }
        ```
        ```c++
        // MyString.h
        class MyString
        {
        public:
            MyString& operator=(MyString&& other);
            // ...
        private:
            char* mString;
            int mSize;
        };
        ```
        ```c++
        // MyString.cpp
        MyString& MyString::operator=(MyString&& other)
        {
            if (this != &other)
            {
                delete[] mString;

                mString = other.mString;
                mSize = other.mSize;

                other.mString = nullptr;
                other.mSize = 0;
            }
            return *this;
        }
        ```
        ```c++
        // Main.cpp
        MyString studentName("Lulu");           // studentName = "Lulu"
        MyString anotherStudentName("Teemo");   // anotherStudentName = "Teemo"

        anotherStudentName = std::move(studentName);   // anotherStudentName = "Lulu", studentName = NULL
        ```

- #### C++11 이후
    - C++11 이후, STL 컨테이너에 이동 생성자와 이동 대입이 생김
    - 그래서, 그것들을 따로 구현할 필요가 없다.
    - C++11 이후의 이동 대입이 적용되어 복사가 일어 나지 않는다.

- #### rvalue 최적화
    - 우리를 흥분시킨 또 다른 C++ 프로그래밍 유행어
    - 맞음, 이번에도 우리는 이걸 잘못 사용했음
    - 이동 생성자와 이동 대입 연산자
        - 아직 유효
    - 포인터 대신 개체 자체를 반환하는 함수
        - 함수에서 rvalue를 반환하는 것으 실제 매우 느림
        - 반환 값 최적화(Return Value Optimization)라고 하는 컴파일러 최적화를 깨뜨림
    - 단, rvalue가 나와야 했던 이유로 
    - 유니크 포인터 같은 경우 move()가 lvalue를 rvalue로 바꾸어 주어야만
    - 데이터 복사 없이, 메모리 할당 없이 이동 생성자, 이동 대입 연산자를 이용해서 
    - 다른 애가 가지고 있는 데이터를 훔쳐오고 걔를 텅텅비게 만들 수 있기 때문이다.
    - 즉, unique_ptr 때문에 rvalue 들어왔다.
    - 베스트 프랙티스
        - 기본적으로 함수 반환할때는 지금까지 하던대로 하고
        - rvalue 같은 쵲거화 작업하지 말아라
        - 컴파일러가 알아서 최적화 해준다.
        - 더 빨라진다고 입증된 경우에만 함수가 rvalue를 반환하도록 바꾸자!
