[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 포인터 1

### 주소 연산자 &
- num이란 변수가 있으면 &num은 그 변수가 위치한 메모리 주소이다.
- 보통 주소를 보여줄 때는 16진수를 사용
  - 16진수는 4bit로 표현, (4bit+4bit) * 4 = 32bit = 4byte, **00 5A F7 AC**
  - 그래서 printf()에서 서식 문자 %p는 주소를 16진수로 보여준다.
  - 참고: 실행 할 때마다 주소가 달라질 수 있음
  
### 포인터(pointer)
- **메모리 주소를 저장하기 위한 변수형 `int*`**
- **메모리 주소를 저장하는 변수 `포인터`**
- 포인터 변수를 선언하는 법
  - 자료형에 **`*`** 을 붙인다.
    - 포인터 변수를 선언할 때는 그 주소에 어떤 형의 데이터가 있는지 명시하기 위해 포인터 앞에 자료형을 붙인다.
  - **`int*`**  : 주소를 따라가면 int형 자료가 있음, 해당 주소에서부터 4바이트를 읽어라.
  - **`char*`** : 주소를 따라가면 char형 자료가 있음, 해당 주소에서부터 1바이트를 읽어라. 
  ```c
  void save_address(void)
  {
    int num = 10;
    int* num_address = &num;
    
    // 보통 num_address를 'int 포인터'라고 부른다.
    // 영어로는 int로의 포인터(pointer to an int)
  }
  ```
  - 리틀 엔디언
    - 데이터가 끝나는 마지막 단위가 가장 작은 메모리 주소에 위치하는 저장 순서
    - 값: 0x004ffaf8 `->` 메모리: f8 fa 4f 00
    - 리틀 엔디언 저장 순서를 여드름이라 상상하면 이해 빠르다.

### 역 참조 연산자 *
```c
const int result = num1 * num2;   // 곱하기 연산자

printf("num : %d\n", *num);       // 역 참조 연산자
```
- **참조** 는 메모리 주소로 접근하는 것이고
- **역참조** 는 메모리 주소의 값에 접근하는 것이다.
  - 포인터는 메모리 주소를 저장하는 변수
  - 그러므로 포인터 변수에 역참조 연산자를 붙이면
  - **변수에 담긴 메모리 주소가 가리키는 값을 가져오는 역할을 하는 것이다.**

### 포인터 변수 선언 vs 역 참조
- 헷갈리지 말자!
```c
int score = 100;

int* pointer = &score;  // 포인터 변수 선언
*pointer = 50;          // 역 참조
```

### 포인터와 함수 반환값
- 당연히 포인터도 변수니깐 함수 반환값으로 사용 가능
```c
int* do_something(const int op1, const int op2);
```
- 다만, 포인터를 반환할 때 조심해야 할 것이 있다.
- **지역 변수의 주소를 반환하는 것은 매우 위험한 코드**
  - 함수의 지역 변수는 스택에 저장된다.
  - 함수 호출이 끝나면 지역 변수도 사라짐
  - 그래서 포인터가 유효하지 않은 주소를 가리키게 된다.
  - 이러한 포인터를 댕글링 포인터라고 한다.
  - 절대 작성해서는 안 되는 코드
- 포인터를 반환해도 되는 경우
  - 전역 변수
  - 파일 속 static 전역 변수
  - 함수 내 static 변수
  - 힙 메모리에 생성된 데이터
- 언제 포인터를 반환할까?
  - 도우미 함수 안에 생성한 변수를 다른 함수에서 사용하고자 할 때
    - 단, 일반 지역 변수면 안됨(함수 호출이 끝나면 스택에서 사라짐)
  - 함수 안에서 대용량의 데이터를 생성하고 그걸 반환하고자 할 때
    - 이 경우에는 데이터를 스택 메모리가 아니라 힙 메모리라는 곳에 생성

### 널(NULL) 포인터
- 아무것도 가리키지 않는 포인터
  - 값이 **`0`인 정수** 상수 표현식
    - 0은 사용하지 않는다. 별로인 코드
  - **`void*`** 로 캐스팅된 표현식
  - 전용 매크로가 있음
    - **`#define NULL ((void*)0)`**
    - **널 포인터를 표현할 때 이 매크로를 사용할 것**
- 반환할 주소가 없는 경우에는 NULL 포인터를 반환한다.
- 포인터 변수와 NULL은 비교(==, !=) 가능
```c
int* ptr;

if (ptr == NULL)    // NULL 대신 0은 사용하지 않는다. 별로인 코드
{
}

if (ptr != NULL)
{
}
```
- 함수 매개변수로 포인터가 들어올 때는 언제나 골칫덩어리
  - 왜냐하면 누구나 NULL을 넣을 수 있기 때문
  - 기본적으로 NULL이 안 들어온다고 가정하고 함수를 작성할 것
  - NULL이 들어올 수 있는 함수는 매개변수명에서 분명히 밝힐 것
  ```c
  // 코딩 표준: 널 포인터를 허용하는 매겨변수
  // 매겨변수 이름 끝에 `_or_null`을 붙인다.
  
  int get_score(const char* cosnt student_id_or_null)
  {
  }
  ```
- NULL이 안 들어온다고 가정한 경우 assert()를 사용해 검증
```c
#include <assert.h>
#define PRICE (2)

void increase_price(int* current_price)
{
  assert(current_price != NULL);
  
  *current_price += PRICE;
}
```
- 기본적으로 NULL을 반환 안 함
  - 함수가 널 포인터를 반환할 수 있다면,
  - 함수 이름 끝에 '_or_null'을 붙인다.
  ```c
  const char* get_name_or_null(const int id)
  {
    // 코드 생략
    return NULL:
  }
  ```
- 널 포인터는 언제 사용하나
  - 포인터 변수를 초기화하고 싶을 때
    - 아직 참조할 주소가 없을 때
    - `int* ptr = NULL;`
  - 포인터 변수가 유효한 주소를 참조하고 있는지 확인하고 싶을 때
    - 역 참조를 하기 전에 널 포인터인지 확인할 것
    ```c
    if (pter != NULL)
    {
      *ptr = 100;
    }
    ```
  - 댕글링 포인터를 막기 위해
    - 동적 메모리 할당된 메모리를 더 이상 필요 없어서 해제했는데
    - 이를 여전히 가리키는 포인터가 있다면?
    - 더 이상 사용할 수 없는 데이터이기 때문에 
    - 포인터 변수에 저장되어 있는 그 주소를 초기화해야 함
    - 이 때 널 포인터를 이용해 리셋
    ```c
    // 동적 메모리 할당
    int* ptr = (itn*)malloc(sizeof(int));
    
    // 코드 100줄
    
    // 더 이상 ptr을 사용하지 않음
    free(ptr);
    ptr = NULL;
    ```

### 포인터의 비교
- 주소를 비교하는 코드
```c
void do_something(int* num1, int* num2)
{
  if (num1 == num2)   // 주소 비교
  {
  }
}
```
- 값을 비교하는 코드
```c
void do_something(int* num1, int* num2)
{
  if (*num1 == *num2)   // 값 비교
  {
  }
}
```
- NULL 외의 주소를 비교하는 이유는
  - 변수 하나가 아니라 큰 메모리 통째를 잡아두고
  - 그 안에 복수의 데이터를 넣어 사용할 때 필요

### 포인터의 크기
- 모든 포인터는 동일한 크기를 가진다.
- 포인터의 크기는 코드를 컴파일하는 시스템 아키텍쳐에 따라 결정
  - 32비트 아키텍쳐에서는 포인터 크기는 4바이트
  - 64비트 아키텍쳐에서는 포인트 크기는 8바이트

### 포인터와 배열
- 포인터에 배열의 시작 주소 대입하는 방법
  - **배열의 이름**
    - 배열의 이름 자체가 시작주소이다.
    - 그래서 포인터에 바로 대입할 수가 있다.
  - **배열의 첫 번째 요소의 주소**
    - 주소 연산자 &를 사용해 첫 번째 요소의 주소를 포인터에 대입
  ```c
  int nums[6] = {0, 1, 2, 3, 4, 5};
  int* ptr1 = NULL;
  int* ptr2 = NULL:
  
  ptr1 = nums;      // 배열의 이름을 곧바로 포인터에 대입
  ptr2 = &num[0];   // 주소 연산자 &를 사용해 첫 번째 요소의 주소 대입
                    // ptr1, ptr2 동일한 결과
  ``
- 매개변수로 받은 배열
```c
void print_scores(int scores[])
{
  size_t size = sizeof(scores);
  
  // scores는 my_scores의 시작 주소를 가지고 있다.
}

ptrint_scores(my_scores);
```
- 매개변수로 받은 포인터
```c
void print_scores(float* dollar)
{
  size_t size = sizeof(dollar);
  
  // dollar는 my_money의 (시작)주소를 가지고 있다.
}

ptrint_scores(&my_money);
```
- 포인터에 정수 1을 더한다는 것
  - 포인터의 위치를 다음 데이터의 위치로 이동
  - 1 바이트를 더하는 게 아니다.
  ```c
  int* ptr = nums;  // ptr: 0x100
  ptr = ptr + 3;    // ox100 + 4 + 4 + 4
                    // int의 사이즈를 세 번 더함
                    // = 0x10c
                    // int*가 아니라 short*였다면 2바이트씩 증가
  ```
  - 그래서 이 두 코드는 같은 의미가 된다.
  ```c
  int* ptr1 = nums + 3;   // ptr1는 nums[3]을 가리킨다.
  int* ptr2 = &nums[3];   // ptr2는 nums[3]을 가리킨다.
  ```
- **배열 요소에 포인터로 접근하기**
  - 배열명은 시작 주소이기 때문에 포인터 변수에 대입할 수 있다.
    - `int* ptr = nums;`
  - 근데 배열의 첨자 연산자`[ ]`도 포인터에 쓸 수도 있다
    - `printf("%d, %d, %d\n", nums[1], ptr[1], *(ptr + 1));`
  - **`nums[1] == ptr[1] == *(ptr + 1)`**
  - 이 모두 컴파일러에게 똑같은 의미
  - 포인터 산술 연산에도 배열 첨자 연산자에도 동이랗게 적용됨
- 포인터에 들어가는 값은 주소
  - C에서는 이 주소를 얻을 수 있는 방법은 딱 두가지 뿐이다.
    - 주소 연산자(&)
    ```c
    float pi = 3.14f;
    float* p = &pi;
    ```
    - 배열의 이름
      - 배열의 이름은 배열의 시작 주소를 알려줌
      ```c
      int days[] = {1, 2, 3, ... , 30, 31};
      int* p = days;
      ```
- 포인터에 정수를 더하면 주소 이동
  - 포인터에 정수 n을 더하거나 빼면 언제나
  - "sizeof(자료형) * n"한 만큼 메모리 주소 이동
  ```c
  char* char_ptr = char_array;  // 0x100
  char_ptr = char_ptr + 10;     // 0x10A
  
  int* int_ptr = int_array;     // 0x100
  int_ptr = int_ptr + 10;       // 0x128
  ```
- 정말 딱 '한' 바이트만 옮기고 싶다면
  - 한 바이트짜리 포인터로 캐스팅
    - `int_ptr = (char*)int_ptr + 1;`
    - **1 바이트만큼 이동 후 거기서 4바이트를 읽음**
  - **`int* -> char*` 캐스팅은 무엇을 캐스팅 하는 걸까?**
    - 그 메모리 주소에 들어 있는 값을 캐스팅하는 것이 아니라
    - 그 메모리 주소에 어떤 형(type)이 들어 있는지 알려주는 것이다.
    - 캐스팅 후 실제 데이터 내용이 char*이 되는 것이 아니다.
- 두 주소 간의 사칙연산
  - 뺄셈을 제외한 사칙연산은 모두 지원안한다.
  - 뺄셈의 경우 두 주소 사이에 들어갈 수 있는 데이터 수를 반환
    - 따라서 포인터가 아니라 정수를 반환
    ```c
    int sub = &nums[5] - &nums[1];  // 4, 배열의 데이터 개수    
    ```
  - 배열의 첫 번째 및 마지막 요소의 주소를 알면 배열의 크기를 구할 수 있다.
  










