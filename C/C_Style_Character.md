[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 메모리 주소
### x86(32bit) 운영체제 용
- 32개의 비트
    - `0000 0000 0000 0000 0000 0000 0000 0000 ~ 1111 1111 1111 1111 1111 1111 1111 1111`
    - 4,294,967,296 (약 43억)
- 4,294,967,296개의 주소를 가릴킬 수 있다.
- ![img](Img/address.png)
- 메모리 한칸은 1바이트의 크기, 즉 **비트 하나의 크기는 1바이트가 된다.**
- 이는 1바이트 크기의 메모리가 4,294,967,296개 까지 인식이 가능하다는 것
- 즉, 메모리의 최대 크기는 4,294,967,296 = 4GB이다. 
- 16진수로 표현
    - `0x00000000 ~ 0xFFFFFFFF`
    - `0xFF00abcd`위치의 주소가 1바이트 증가하면 `0xFF00abce`
- ![img](Img/address3.png)

# C-Style 문자열
### char[]로만 구성
- 문자열이 끝나는 곳에 널 문자(`\0`)를 붙인다
- 문자열 뒤에 별도로 `\0`를 넣지 않아도 컴파일러가 알아서 넣어준다.
    - `char str1[] = "abc";  // 스택에 "abc" 저장`
    - `char* str2 = "abc";   // 테이터 섹션에 "abc" 저장`
        - **자신이 바꾸지 않을 문자라면 이렇게 해도 상관없다.**
- 단, 이 경우는 `\0`을 넣어주지 않음
    - `char str[] = { 'a', 'b', 'c' };`
    - 문자열을 만드는데 배열 요소마다 지정해주면 컴파일러가 널 캐릭터를 만들어주지 않는다.

### 보통 C-Style 문자열이라 하면 널 문자(null character)로 끝나는 char 배열을 말한다.
- **널 문자**(null character, 널 캐릭터)
- 아스키코드 중에 화면에 출력되지 않는 특수 문자들이 있음
    - `0~31`, `127`
    - 그 중에 하나가 바로 `0`
    - `0`은 숫자 영
    - `\`는 이스케이프 문자
    - 근데 아스키코드로 `0`이니 `char null_char = 0`으로 작성 가능
    - **그.러.나** 읽기 쉽게 `\0`으로 써주자
- **널 포인터하고 다름** ; 헷갈리지 말자!

### 문자열의 길이는 3, 배열의 길이는 4
- `const char str[] = "abc";`
    - `|'a'|'b'|'c'|'\0'|`
    - 문자열의 길이는 3;
    - 배열의 길이는 4;
- 효율적인 문자열 길이 구하기
```c
#include <stdio.h>

size_t get_string_length(const char* str) // 읽기 전용으로 str 시작주소를 받음
{
    const char* p = str;    // const char* p는 그 주소가 가리키는 값을 보호  

    while (*p++ != '\0')		
    {
        // 값이 아니라,
        // p++는 포인터 p의 주소를 1만큼 증가 시키는 연산이기 때문에 
        // const char* p는 에러 없이 실행된다.
        // 하지만 *p의 값을 변경하려면 에러가 난다. 
        // 왜냐하면, const char* p는 
        // 그 주소가 가리키는 값의 변경을 보호하기 때문이다.
        // 참고로, int* const p = str;은 포인터 p가 가리키는 주소를 보호한다.
    }
    return p - str - 1;	  // 두 주소 간의 사칙연산은 뺄셈만 지원한다.
}

int main(void)
{
    char str[] = "abcd";  // 00 ef f8 14

    size_t count = get_string_length(str);

    printf("count: %d", count);

    return 0;
}
```

### C-Style 문자열의 장단점
- 장점
    - 가장 최소한의 메모리!
    - 한 가지 데이터형으로 문자열과 길이를 다 표현!
- 단점
    - 어떤 문자열의 길이를 알려면 배열을 끝까지 훑어야 함. O(N)


# C 문자열 함수들
### `<string.h>`에 있는 문자열 함수들
- strlen()
- strcmp() / strncmp()
- strcpy() / strncpy()
- strstr()
- strcat() /strncat()
- strtok()
- 그 외 다수 

### 특징
- 꽤 많은 함수들이 문자열을 절대 변경하지 않는다!
    - 매개변수에 **`const char*`** 붙인다.
- 문자열을 변경하더라도 원본은 변경 안 하려 한다!
    - 사본만 변경
    - 예외: strtok()
- 절대 새로운 문자열(즉, 연속된 char 메모리)을 만들어 주지 않는다!
    
### strlen() : 문자열 길이 구하는 함수
```c 
size_t strlen(const char* str);
```
- 널캐릭터 없는 문자열을 무작정 읽으면 위험한 일이 일어날 수 있다.
- 그래서, 외부에서 들어오는 문자열 읽을 때 조심해서 읽어야 한다.
- C11의 **`strlen_s()`** 함수가 이 문제를 해결하기도

### strcmp() : 문자열 비교 함수
```c 
int strcmp(const char* lhs, const char* rhs);
```
- 사전식 순서로 어떤 문자의 아스키코드가 더 작음/같음/큼을 판별
    - ABCD < ABCE    : return -1;
    - abcd > ABCD    : return 1;
    - ABC < ABCEFG   : return -1;
    - abcd = abcd    : return 0;
- 반환값
    - 같으면 `0`           : return 0;
    - 좌항이 작으면 `< 0`  : return -1;
    - 좌항이 크면 `> 0`    : return 1;
    - 음수도 반환하니 반환형은 `int`
```c
int compare_string(const *char str0, const char* str1)
{
    while (*str0 != '\0' && *str0 == *str1)
    {
        ++str0;
        ++str1;
    }
    
    return *str0 - *str1;
}

int compare_string2(const *char str0, const char* str1)
{
    while (*str0 != '\0' && *str0 == *str1)
    {
        ++str0;
        ++str1;
    }
    
    if (*str0 == *str1)
    {
        return 0;
    }
    
    return *str0 > *str1 ? 1: -1;
}

const char* msg1 = "AB";
const char* msg2 = "AC";
int result = compare_string(msg1, msg2);
int result2 = compare_string2(msg1, msg2);
int result3 = strcmp(msg1, msg2);
```

### strcpy() : 문자열 복사
```c
char* strcpy(char* dest, const char* src);
```
- 위험할 수 있는 함수
- dest 크기 < src 크기
    - 잘못된 메모리 쓰기 발생
- 두 크기를 확실히 통제 가능하다면 안전

### strncpy() : 비교적 안전한 문자열 복사
```c
char* strncpy(char* dest, const char* src, size_t count);
```
- 최대 count만큼 복사
- 널 문자(`\0`)를 먼저 만나면 그전에 끝냄
- src가 count보다 짦으면
    - 남은 걸 다 0(`\0`)으로 채워줌
    - dest |H|i|\0|\0|\0|
- src가 count보다 길거나 같다면
    - count만큼 복사함
    - 널 문자를 붙일 곳이 없음
    - 따라서 안 붙여줌
        - dest |L|u|l|
    - 그래서 프로그래머가 언제가 이렇게 코드 한줄을 추가
        - dest |L|u|\0|
    ```c
    strncpy(dest, src, DEST_SIZE);
    dest[DEST_SIZE -1] = '\0';
    ```    
- strcpy()보다 안전
- 덜 빠름
    - dest의 남은 요소를 0으로 채우기 때문
- 여전히 위험한 경우가 있음
    - count보다 src가 길 경우
        - 다 복사하고 널 문자가 없음
        - 프로그래머가 널 문자를 붙여줘야 함
- C11에서 이보다 안전한 strcpy_s(), strncpy_s() 함수가 있음!

### strcat() : 문자열 합치기
```c
char* strcat(char* dest, const char* src);
```
- src의 문자열을 dest 뒤에 덧붙이는 함수
    - dest의 널 문자가 들어있는 위치부터 src의 문자열 추가
    - 바꿔 말하면 dest의 널 문자가 src[0]로 교체
- dest의 길이가 충분해야 함
    - 이 길이를 쓸 경우 정의되지 않느 결과 발생
    - dest 범위를 넘어선 곳까지 데이타가 복사

### strncat() : 비교적 안전한 문자열 합치기
```c
char* strncat(char* dest, const char* src, size_t count);
```
- count 개의 문자를 복사한 뒤, 널 문자를 가장 마지막에 붙여줌
- 따라서, 최대 count + 1 개의 문자를 덮어 씀
- dest의 길이보다 길게 쓰면 마찬가지로 정의되지 않은 결과 발생
    - 그러나 count로 이러한 경우가 발생하지 않도록 프로그래머가 제어 가능
    - 따라서 strcat()보다는 조금 더 안전
    ```c
    #deifn DEST_COUNT (20)
    
    const char* src = "HELLO";
    char dest[DEST_COUNT] = "Hi ";
    
    strncat(dest, src DEST_COUNT - strlen(dest) - 1);
    ```
- C11에서 이보다 안전한 strcat_s(), strncat_s()함수가 있다!

### strstr() : 문자열 속에 문자열 찾기
```c
char* strstr(const char* str, const char* substr);
```
- 반환값
    - cahr 포인터
    - substr이 str에 있다면 : 해당 substr이 시작하는 주소
    - substr이 str에 없다면 : 널 포인터(NULL)
- 반환값으로 메모리 주소를 돌려주는 이유
    - 새로운 문자열을 만들어 반환할 경우 메모리 관리 측면에서 효율적이지 못하고 실수할 수 있음
- 그럼 어디에 그 새로운 문자열을 저장할까?
    - 새 문자열을 반환하려면 메모리 '어딘가'에 그 문자열을 복사해야 한다.
    - 복사하는 위치가 스택이면
        - 함수 끝나면 사라짐 -> 반환값이 더 이상 유효하지 않은 메모리 주소
    - 복사하는 위치가 힙이면(동적 메모리 할당)
        - 메모리 할당을 운영체제에게 부탁해야 하므로 느림
        - 그리고 더 이상 사용 안 할 경우 프로그래머가 직접 메모리 해제 함수를 호출해야 하는데 깜박 잊고 안할 수도 있음
    - 그래서 그냥 원본에서 찾고자 하는 문자열이 시작하는 주소를 반환하는 걸로 간단하게 해결
        - 추가적으로 메모리를 쓰지도 않고
        - 사람이 저지를 수 있는 실수도 줄일 수 있다.
```c
#include <string.h>

int main(void)
{
    char msg[] = "I love string! I love C!";
    
    char* result = strstr(msg, "string");
    printf("result: %s\n", result == NULL ? "(null)" : result);
    
    return 0;
}
```


























