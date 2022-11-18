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























