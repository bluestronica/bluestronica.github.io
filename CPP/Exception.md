### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 예외(Exception)
- #### C++에서의 예외(Exception)
    - C++도 예외를 "지원"
    - 허나, C++에서는 예외의 중요성이 좀 떨어짐
    - 기본 옵션으로 예외처리 기능이 꺼져 있는 C++ 컴파일러들이 많다.
    - 그래서 C++도 예외처리를 지원할 수 있다는 정도만 기억하자.

- #### 웹 애플리케이션에서의 에러 처리
    - Struct, Class를 통해 C++에도 적용 가능
    - 함수에서 **상태**와 **값**을 반환
        ```c++
        // Error.h
        enum EError
        {
            SUCCESS,
            ERROR,
        };

        struct ErrorCode
        {
            EError Status;
            int Code;
        };

        template <typename T> struct Result
        {
            ErrorCode Error;
            T Value;
        };
        ```
        ```c++
        // Main.cpp
        Result<const char*> result;

        result = record.GetStudentIDByName("Blues Tronica");

        if (result.Error.Status == ERROR)
        {
            cout << "Error Code" << result.Error.Code << endl;
        }
        else
        {
            count << "Student ID is " << result.Value << endl;
        }
        ```

- #### 적절한 예외 처리 전략
    - 유효성 검사/예외는 **오직 경계에서만**
        - 밖에서 오는 데이터를 제어할 수 없기 때문
        - 예) 외부에서 들어오는 웹 요청, 파일 읽기/쓰기, 외부 라이브러리
    - 일단 시스템에 들어온 데이터는 다 올바르다고 간주할 것
        - **assert**를 사용해서 개발 중 문제를 잡아내고 고칠 것
            ```c++
            int ConvertToHumanAge(const Animal* pet)
            {
                Assert(pet != NULL);
                // ...
            }
            ```
    - 예외 상황이 발생할 때는 NULL을 능동적으로 사용할 것
        - 하지만 기본적으로 함수가 NULL을 반환하거나 받는 일은 없어야 함
        - 코딩표준: 만약 NULL을 반환하거나 받는다면 함수의 이름을 잘 지을 것
            ```c++
            // 매개 변수가 NULL이 될수 있으면 매개 변수 이름 뒤에 'OrNull'을 붙인다.
            const char* GetCoolName(const char* startWithOrNull) const;

            // 함수가 null을 반환할 수 있으면, 함수 이름 뒤에 'OrNull'을 붙인다.
            const char* GetHobbyOrNull() const;
            ```