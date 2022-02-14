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
        for (std::vector<float>::cosnt_iterator it = scores.begin(); itn != scores.end(); ++it)
        {
            // ..
        }
        return percentages;    // 복사 일어남
    }

    // Main.cpp
    int main()
    {
        std::vector<float> scores;
        // ...
        scores = ConvertToPercentage(scores);  // 복사 일어남
        // ...

        // 두 번의 복사가 일어남!!!
    }
    ```

- #### rvalue와 복사 생성자