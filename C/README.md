[bluestronica.github.io](https://bluestronica.github.io/)

### [Bitwise Operators In C](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Bitwise_Operators_In_C.md)
- 비트 연산자
- 시프트 연산자
- 정리
- 비트필드

### [기본 문법 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Basic.md)
- main(void) 함수: (void)
- 데이터가 메모리에 저장되는 방식
- 기본 자료형(primitive types)
- 연산자
- 조건과 반복

### [기본 문법 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Function.md)
- 함수(Function)
- 평가 순서
- 범위(scope)
- const 베스트 프랙티스
- 배열(array)

### [기본 문법 3 Build](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Build.md)
- C프로그램의 빌드 과정
- 정리

### [포인터 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Pointer1.md)
- 주소 연산자 &
- 포인터(pointer)
- 역 참조 연산자 *
- 포인터 변수 선언 vs 역 참조
- 포인터와 함수 반환값
- 널(NULL) 포인터
- 포인터의 비교
- 포인터의 크기
- 포인터와 배열

### [포인터 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Pointer2.md)
- 포인터와 배열의 차이
- 포인터 산술 연산
- 포인터와 const
- 포인터의 용도
- 포인터 배열

### [C-Style Character](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/C_Style_Character.md)
- x86(32bit) 운영체제 용
- C-Style 문자열
- char[]로만 구성
- 보통 C-Style 문자열이라 하면 널 문자(null character)로 끝나는 char 배열을 말한다.
- 문자열의 길이는 3, 배열의 길이는 4
- C-Style 문자열의 장단점
- C 문자열 함수들
- `<string.h>` 에 있는 문자열 함수들
- 특징
- strlen() : 문자열 길이 구하는 함수
- strcmp() : 문자열 비교 함수
- strcpy() : 문자열 복사
- strcpy() : 비교적 안전한 문자열 복사
- strcat() : 문자열 합치기
- strncat() : 비교적 안전안 문자열 합치기
- strstr() : 문자열 속에 문자열 찾기
- strtok() : 문자열을 토큰화하기 

### [Inuput / Output](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Input_Output.md)
- 출력 (Output)
- 입력 (Input)
- 파일 입출력
- 스트림의 위치
- 입출력 리디렉션(IO redirection)

### [getchar(), getche(), getch(), putchar(), putch()](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/getchar_getche_getch.md)
- getchar(), getche(), getch()
- putchar(), putch()

### [구조체 / 공용체](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Structure_Union.md)
- C에서 구조체는?
- 구조체 선언
- 새로운 별명을 지어주는 typedef
- 구조체에 typedef 적용
- 커스텀 자료형에 typedef를 쓰자! 
- 구조체 포인터에 멤버의 값에 접근하기
- 구조체 매개변수 베스트 프랙티스
- 함수 반환값으로서의 구조체
- 구조체로 배열도 만들수 있다.
- 얕은 복사
- 깊은 복사
- 파일을 읽고 쓸 때도 비슷한 문제가 발생한다.
- 그래서 포인터 대신 배열로 구조체 작성
- 바이트 정렬 요구사항
- 구조체와 비트 플래그
- 공용체
- 똑같은 메모리 위치를 다른 변수로 접근하는 방법
- 공용체 안에 있는 여러 변수들이 같은 메모리를 공유 

### [함수 포인터](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Function_Pointer.md)
- 함수도 어디다 저장해 둔 뒤 매개변수로 전달할 수 없을까?
- 함수 포인터 선언
- 함수 포인터 변수의 선언과 사용
- 함수 포인터 매개변수의 선언과 사용
- 함수 포인터 읽는 방법
- 포인터 함수를 담는 배열
- void*

### [가변 인자 함수](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Variadic_Function.md)
- va_arg()는 매크로 함수
- 어떻게 다른 수의 매개변수를 스택에 넣을까?
- 첫 번째 매개변수가 저장된 스택 메모리 위치는?
- 가변 인자 함수는 이렇게 인자를 읽어온다!
- 

### [C에서 올바른 오류처리 방법](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Error_Handling.md)
- C에서 오류처리
- 어서트
- 널 포인터를 허용한다면 함수나 변수에 명시!
- 실행 중 오류 처리는?
- 오류 코드를 반환하자!
- 모든 오류 코드를 하나의 enum으로 만들자!
- 올바른 오류 처리 전략 정리
- 그런데도 프로그램이 오작동하면?

### [레지스터, 스택 & 힙](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Memory.md)
- 레지스터(register)
- x86 아키텍쳐에서 사용하는 레지스터
- 스택 메모리의 단점 - 수명
- 스택 메모리의 단점 - 크기
- 힙 메모리
- 힙 메모리의 장점
- 힙 메모리의 단점
- 정적 메모리 vs 동적 메모리


### [동적메모리, 다중 포인터](https://github.com/bluestronica/bluestronica.github.io/blob/main/C/Memory2.md)
- 프로그램이 동적 메모리를 가져다 사용할 때 총 세 가지 단계를 거친다.
- 메모리 할당 함수: malloc()
- malloc()의 짝꿍 함수 free()
- 할당받은 메모리를 해제하는 함수: free()
- 할당받아 온 주소를 그대로 연산에 사용하면?
- 할당받은 포인터로 연산 금지!
- 메모리 해제 후 널 포인터를 대입
- free()는 몇 바이트를 해제할 지 어떻게 알까?
- 메모리 할당 함수: calloc()
- 메모리 할당 초기화 함수: memset()
- 메모리 재할당 함수: realloc()
- 메모리 복사 함수: memcpy()
- 메모리 누수 안 나게 코드를 작성할 것!
- realloc()의 특수한 경우
- 메모리 비교하는 함수: memcmp()
- 구조체 두 개를 비교할 때 유용
- 구조체 멤버 변수 - 배열 vs 포인터
- 정리
- 다중 포인터
- 이중 포인터 사용






