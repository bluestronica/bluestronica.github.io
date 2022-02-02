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
        int& reference = number2;  // number1 = reference = number2 = 200;
                                // 즉, reference 값의 변경은 number1의 값 변경으로 이루어진다.

        // error
        int& reference = NULL;   // NULL이 될 수 없다.
        int& reference;          // 초기화 중에 반드시 별칭이 선언되어야 한다.
        ```

    - 함수 매개변수로서의 참조
        ```C++
        ```