[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 기본문법(Basic)

### main(void) 함수: (void)
- **C에서 함수선언과 함수정의에서 매개변수가 없으면 언제나 void를 넣는 습관을 기르자!**
    ```c
    int main(void)
    {
        return 0;
    }
    
    static void main(string[] args)
    {
        return 0;   
    }
    ```
    ```c
    int sum(void);  // 함수선언 : 매개변수가 없음
    int sum();      // 함수선언 : 매개변수를 받는다.
    
    int sum(void)   // 함수정의 : 매개변수가 없음
    {
    }
    
    int sum()       // 함수정의 : 매개변수가 없음
    {
    }
    
    int sum(const int num1, const int num2)   // 함수정의 : 매개변수가 2개 있음
    {
    }
    ```
  

### 데이터가 메모리에 저장되는 방식
- C에서 다루는 데이터(-, +, 정수, 실수, 문자, 문자열)가 컴파일 된 후에는 2진수 표기법으로 컴퓨터 안에서 값이 저장되어 표현된다.
- 테이터의 값을 바꿀 수 없는 상수의 형태와 값을 바꿀 수 있는 변수의 형태가 있다.


### 기본 자료형(primitive types)
- unsigned, signed
    - C는 unsigned, signed라는 단어를 자료형 이름 앞에 넣어줘야 함
    - unsigned, signed를 생략하면 부호있음(signed)이 된다.
        - int default_int = -10000;
    - 부호가 없을 때, unsigned를 사용한다.
        - unsigend char unsigned_char = 255;
        - 포팅 문제 없는 범위 = 0~127
        - unsigned int unsigned_int = 23456;   

- char
    - 크기 : 8비트
    - 부호(unsigned/signed)를 생략할 경우 : signed
        - char signed_char = -1;
    - char로 표현 가능한 숫자의 범위(표준)
        - 부호 없는 경우(unsigned char) : 0~255
        - 부호 있는 경우(signed char) : -128~127
    - char의 부호 여부를 판단하는 방법
        - `<limists.h>`헤더 파일에서 CHAR_MIN을 보면 **부호 식별자가 없는 char**가 signed인지 unsigned인지 알 수 있음

- char와 ASCII 문자
    - char는 ASCII 문자를 표현하기에 충분하다
    - ASCII는 0~127인 숫자이다 
    - 덧셈 가능
    ```
    char ch_a = 'a';        // 97
    char ch_b = ch_a + 1;   // 98
    char ch_c = 99;         // 99
    
    // 값 97이 컴파일되면 
    // 2진수 표기법으로 char 크기(8비트)만큼 공간을 차지하고 값이 저장된다.
    // 정수로 출력하면 97, 문자로 출력하면 'a'(ASCII 값 97이 'a'로 대응) 
    ```

- short
    - 최소 16비트이고 char의 크기 이상인 정수
    - 기본 정수형 int 보다 짧음
    - 그러나 int 대신 short를 사용할 경우 성능이 느려질 수도 있다.

- int
    - int는 기본 정수(integer)
    - 크기 : 32비트(4바이트)
    - 범위
        - 부호 없는 경우(unsigned) : 0~4,294,967,295
        - 부호 있는 경우(signed) : -2,147,483,648~2,147,483,647
        - 부호 있는 수의 최댓값보다 큰 값을 unsigned int에 대입할 경우 int의 리터럴 'u' 혹은 'U'를 붙여야 함
        ```
        int signed_int = -1024;
        
        unsigned int unsigned_int1 = 394;
        unsigned int unsigned_int2 = 2147483648;     // 경고
        unsigned int unsigned_int3 = 2147483648u;    // 경고 없음, 대문자 U도 
        ```

- long
    - int가 16비트일 때 그것보다 2배 큰 자료형이 필요했음
    - 따라서, long은 최소 32비트이고 int 이상의 크기를 가지니다.
    - 다른 언어에서는 long이 보통 64비트
    - C89에는 64비트 정수형은 없음
    - 포팅 안전한 범위 : -2,147,483,648~2,147,483,647    
    ```
    unsigned long unsigned_int2 = 2147483648;      // 경고
    unsigned long unsigned_int3 = 2147483648ul;    // 경고 없음
    
    // l은 long
    // u는 unsigned
    // ul은 unsigned long
    ```

- float
    - 부동 소수점 자료형
    - 크기는 32비트
    - 범위는 IEEE754 Single
    - 실수는 소수점 형태와 지수 형태로 표현할 수 있다.
        - 소수점 형태
            - 0.0000314
        - 지수 형태
            - 3.14*10^-5
            - **C언어 표기법**
                - 3.14e-5
                - 0.314E-4
    - unsigned형 없음
    - float의 리터럴은 3.14f

- double
    - 부동 소수점 자료
    - 크기는 64비트
    - 범위는 IEE754 Double
    - unsigned형 없음

- bool 형을 안쓰는 이유
    - 정수로 대신 쓸 수 있음
    - 0이면 false, 0이 아니면 true
    - 하드웨어에서도 실제 bool이 없음
    - 0이냐 아니냐만 있

- **코딩 표준** : 참, 거짓을 반환해야 할 때는
    - 거짓일 때는 0을 반환
    - 참일 때는 1을 반환
    ```
    int is_student(const int id)
    {
        if(/* 조건 : 참 */)
        {
            return 1;
        }
        return 0;
    }
    ```
    
- enum
    - C에서의 열거형은 그냥 정수에 별명 붙이는 수준
    ```
    enum day { DAY_MONDAY, DAY_TUESDAY, DAY_WEDNESDAY, };
    enum month { MONTH_JANUARY, MONTH_FEBRUARY, MONTH_MARCH, };

    enum day hump_day = DAY_WEDNESDAY;
    enum month birth_month = hump_day;  // 컴파일이 된다. 태어난 달이 '수요일'
    ```

- 변수 선언 위치
    - 변수 선언은 반드시 블럭의 시작에서만 해야함
    - 코드 중간에 사용하는 변수는 블록 시작에서 선언하고 뒤에 대입
    ```
    int main(void)
    {
        int num1 = 10;      // 선언 대입
        int num2 = 1234;    // 선언 대입
        int result;         // 선언
        
        printf("num1 = %d, num2 = %d", num1, num2);
        
        result = add(num1, num2);   // 대입
        
        printf("%d + %d = %d", num1, num2, result);
        
        return 0;
    }
    ```

## 연산자
- 연산자 순위
- C에서 새로 만나는 연산자
    - `sizeof()`
        - 함수가 아니다
        - 피연산자의 크기를 바이트로 반환해주는 **연산자**
        - 실행 중이 아니라 컴파일 도중에 크기를 찾음
        - char 형을 넣으면 반드시 1이 반환
        - sizeof() 연산자가 반환하는 값은 부호 없는 정수형의 상수로 size_t 형이다.
        ```
        char ch = 'a';
        char num = 100;
        
        size_t size_char = sizeof(ch);  // 1
        size_t size_int = sizeof(num);  // 4
        ```
        - size_t
            - 부호 없는 정수형이나 실제 데이터형은 아님
            - _t는 typedef(다른 자료형에 별칭을 붙이는 것)를 했다는 힌트
            - 반복문이나 배열에 접근할 때 사용 (음수가 필요없으니까)
            
    - 역 참조 연산자 `*`
    - 주소 연산자 `&`
    - 구조체와 공용 맴버 접근자 `.` 과 `->`
    
## 조건
- 조건 연산자
    - 삼항 연산자라고 부르는게 보통
    ```
    int num1 = 10;
    int num2 = 56;
    int min = num1 < num2 ? num1 : num2;
    ```

- if문과 불 표현식(Boolean expression)
    - 숫자를 if문의 조건식에 넣어도 곧바로 판단이 가능
        - false : 0
        - true : 0이 아닌 것(따라서, 음수도 포함)
    - 메모리 주소(포인터)나 float 형(3.14f)도 마찬가지
        - 모든 비트패턴이 0이면 false 아니면 true
    ```
    float num = 0.0f;
    
    if (num)    // false : 0
    {
        printf("%f\n", num);
    }
    
    
    float num = 3.14f;
    
    if (num)    // true : 3.14f
    {
        printf("%f\n", num);
    {
    ```
    
- switch
    - case에 가능한 데이터형
        - C는 정수형(int, char, enum)만 가능
    - case 안에서 break를 빼먹으면 곧바로 탈출하지 않고 그 아래 있는 코드를 계속 실행
        ```
        enum fruit { FRUIT_APPLE, FRUIT_MANGO };
        enum fruit fruit = FRUIT_APPLE;
        
        siwtch (fruit)
        {
            cae FRUIT_APPLE:
                printf("Breakfast\n");
                /* intentional fallthrough */
                // 의도를 가지고 'break;'를 사용하지 않을 경우에 반드시 주석을 붙이자
            case FRUIT_MANGO:
                printf("Lunch\n");
                break;
            default:
                printf("Unknown food\n");
                break;
        }
        ```
        
- for
    - for문의 초기화 코드에 size_t i = 0을 못 씀
        - 변수 선언은 제일 위에서 해야하기 때문
        ```
        int sum = 0;
        size_t i; 
        
        for (i = 0; i < 10; ++i)    // ++i 전위증감 습관들이기 
        {
            sum += i;
        }
        ```

- while
    - 조건식은 bool형 대신 1, 0을 반환
    - 그래서 그냥 counter 변수를 넣는 경우가 있는데 좋은 습관이 아니다.
    - `== 0` 혹은 `!= 0`을 넣는다.
    ```
    int day = 5;
    while (day-- != 0)
    { 
        printf("%d\n", day);
    }
    ```
    
- for, while, do-while 실행 도중 
    - 탈출하려면 `break`
    ```
    size_t num = 30;
    
    while (num > 0)
    {
        printf("Count down...%d\n", num);
        --num;
        
        if (num < 15)
        {
            break;
        }
    }
    ```
    - 다음 회차로 넘어가려면 `continue`
    ```
    size_t num = 10;
    
    while (num > 0)
    {
        if (num == 5)
        {
            --num;
            continue;
        }
        
        printf("Count down...%d\n", num);
        --num;
    }
    ```

































