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
##    
- **데이터가 메모리에 저장되는 방식**
    - C에서 다루는 데이터(정수, 실수, 문자, 문자열)가 컴파일 된 후에는 2진수 표기법으로 컴퓨터 안에서 값이 저장되어 표현된다.
    - 테이터의 값을 바꿀 수 없는 상수의 형태와 데이터는 값을 바꿀 수 있는 변수의 형태가 있다.
        -























