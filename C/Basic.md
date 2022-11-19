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

- 코딩 표준 : 참, 거짓을 반환해야 할 때는
    - 거짓일 때는 0을 반환
    - 참일 때는 1을 반환
    ```
    int is_student(const int id)
    {
        if(/* 조건 */)
        {
            return 1;
        }
        return 0;
    }
    ```





































