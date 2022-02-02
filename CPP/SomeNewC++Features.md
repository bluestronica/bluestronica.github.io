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

    - 함수 매개변수로서의 포인터
        ```C++
        void Swap(int* number1, int* number2)
        {
            int temp = *number1;
            *number1 = *number2;
            *number2 = temp;
        }
        ```
    - 함수 매개변수로서의 참조
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