[bluestronica.github.io/C](https://bluestronica.github.io/C)

# Function

### 함수 선언과 함수 정의
- **함수 선언**
  - 함수의 구현체 없이 함수 원형만 선언해 주는 것
    - 함수의 이름
    - 반환형
    - 매개변수들의 자료형
  - 위치
    - 함수를 사용하기 전에 그 함수를 선언
    - 보통 파일의 제일 위에
- **함수 정의**
  - 실제로 함수를 구현해 놓은 것
- **함수 선언과 정의를 분리**
```c    
#include <stdio.h>

void foo(void); // 함수 선언; 전방 선언

int main(void)
{
  foo();
  getchar();
  return 0;
}

void foo(void)  // 함수 정의
{
  printf("foo called");
}
```
- **함수 선언과 정의가 하나**
```c    
#include <stdio.h>

void foo(void)  // 함수 정의(+선언)
{
  printf("foo called");
}

int main(void)
{
  foo();
  getchar();
  return 0;
}
```































