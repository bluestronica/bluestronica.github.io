[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 출력 (Output)

### 서식 지정(formatted) 출력
- **C에서 출력을 논할 때 가장 기본이 되는 함수**
    - **`printf()`** : **콘솔창(stdout)에 출력**
        ```c
        const char* msg = "Hello World";

        printf("%s\n", msg);
        printf("Hello World)";
        ```
    - **`fprintf()`** : **스트림에 출력**
        ```c
        const char* msg = "Hello World";

        fprintf(stdout, "%s\n", msg);
        fprintf(stdout, "Hello World\n");
        fprintf(stderr, "Error!!\n");
        ```
    - **`sprintf()`** : **문자열에 출력**
        - **출력은 char 배열에!!**
        - 정말 많이 씀!
        - 심지어 C++에서 string 클래스가 있는데도 이걸 대신 많이 씀
        - 쓰는 이유는 속도 때문(가장 빨리 문자열을 조작하는 함수는 C함수)
        - 다만, 프로그래머가 충분히 큰 버퍼를 잡아주지 않으면 위험!
            - 비교적 안전한 함수 : snprintf()
        ```c
        char buffer[100];
        int score = 100;
        const char* name = "Hello";

        sprintf(buffer, "%s: %d", name, score);

        printf("%s\n", buffer);
        ```
- **서식 문자열이 필요한 이유**
    - 일단 오버로딩 없음 : printf(int), printf(char) 불가능
    - 그리고 임시 문자열 등을 자동으로 생성 안 해줌
    - 즉, 서식 문자열은 추가 메모리 할당 없이 있는 자료형을 출력 스트림에 문자들로 출력해줌!

- **보통 프로그램이 실행할 때 기본적으로 3개의 스트림을 준다.**
    - **`stdout`** (콘솔 출력)
        - 콘솔 스트림
        - 보통 라인 버퍼링(line buffering)을 사용
            - 버퍼링
                - 출력할 내용이 있어도 곧바로 출력하지 않고 쌓아 둠
                - 어느 정도 버퍼가 차면 그제서야 출력
            - 라인 버퍼링 (다음과 같은 경우에 버퍼를 비움)
                - 버퍼가 꽉 차거나
                - 버퍼에 `\n`가 들어 있을 때
            - 버퍼링 없음
                - 버퍼를 사용하지 않음
        - 강제로 버퍼를 비우고 싶다면 fflush(stdout);을 호출하면 된다.
    - **`stdin`** (콘솔 입력)
    - **`stderr`** (콘솔 출력임. 하지만 오류 메시지를 출력하는 스트림)
    - 이 3개의 스트림을 표준 스트림(standard stream)이라 한다.


### 기타 출력 함수
- **puts()**
    ```c
    int puts(const char* str);
    int fputs(const char* str, FILE* string);
    ```
    - **문자열**을 **stdout**에 출력
    - 마지막에 줄도 바꿔줌: `\n`
    - **`fputs(str, stdout)`** 와 매우 비슷

- **putchar()**
    ```c
    int putchar(int ch);
    int fputc(int ch, FILE* stream);
    ```
    - **문자**를 **stdout**에 출력
    - **`fputc(ch, stdout)`** 하고 같음


# 입력 (Input)

### 입력은 출력보다 까다롭다.
- 출력에 비해 조심해야 할 일이 많다.
- 외부의 데이터를 읽어와서 프로그램에 사용
- 어떤 데이터가 들어올지 몰라서 괴상한 데이터가 종종 들어옴
- 모든 입력 함수에는 반환값이 있음
- 따라서 어떤 함수가 어떤 값을 반환하는지 문서에서 확실히 읽고 코드에서 검사할 것

### 입력은 어디서부터 읽어올까?
- 어딘가에 출력을 했다면 읽어올 수도 있다고 생각하면 편함
- **스트림**
    - 콘솔 창에 출력했으니 콘솔(키보드)로부터 입력받아 옴
    - 파일에 출력(저장)했으니 파일로부터 입력(읽어)받아 옴
    - 등등
- **문자열**
    - 문자열에 출력(저장)했으니 문자열로부터 입력(읽어)받아 옴

### 입력처리 전략
- 한 글자씩 읽기
- 한 줄씩 읽기
- 한 데이터씩 읽기 (데이타 타입에 맞게 읽는 방법)
- 한 블록씩 읽기 (이진 데이타)

### 한 글자씩 읽기 - getchar(), fgetc()
```c
int getchar(void);
int fgetc(FILE* stream);
```
- 키보드(stdin)으로 부터 문자 하나를 읽음
- 반환값
    - 성공 시, 읽은 문자(의 아스키코드)를 반환
    - 실패시, EOF를 반환(EOF는 음수, 그래서 int를 반환)
- fgetc(stdin)하고 같음

### 한 글자씩 읽는 알고리듬
1. 한 글자(char)를 읽어온다
2. 글자를 읽어오는데 실패했다면(EOF) 프로그램을 종료
3. 아니라면 그 글자를 필요한 곳에 사용한다
4. 1번 단계로 되돌아 간다
```c
#include <stdio.h>

int c;

// 키보드로 '7'를 입력 후 엔터키 누름('\n')
c = getchar();  // 버퍼로부터 한 글자를 읽어옴 ; 7만 읽어옴
                // 키보드로 '7'을 입력했다면 7의 아스키코드(0x37)가 반환될거임
                // c = 0x37;
while (c != EOF) 
{
    putchar(c);      // 읽은 문자 출력
    c = getchar();   // 아직 버퍼에는 '\n'이 남아 있음
}     


// getchar() 한번으로 줄이기
int c;

while ((c = getchar()) != EOF) // 한 줄로 줄이는 건 실수하기 쉬움!! 괄호빼먹을 수도
{
    putchar(c);
}
```
- EOF 키는 ctrl키와 다른 키를 조합해서 넣음
    - 윈도우 : **ctrl + z**
    - 유닉스 같은 시스템들 : ctrl + d

### 한 글자씩 읽는 방법이 유용한 경우
- 가장 간단한 입력방법!
- 입력이 문자/문자열일 때 매우 좋음
- 쓸데없이 메모리에 입력값을 저정해 두지 않아도 됨
- for문 딱 한 번만 도는 알고리듬에 적합한 경우가 많음
    - 입력값을 모두 읽어 배열에 저장해 두고 for문 돌려 처리
    - 키보드로부터 한 글자씩 읽어서 곧바로 처리
- 그러나 다른 데이터형을 쓰기는 좀 어려움
    -정수형 숫자 1004를 읽기
        - '1','0','0','0','4' 이렇게 4번 읽어서 그걸 정수로 변환하긴 좀....
- O(N)이 되는 알고리듬
    - 어떤 데이터의 개수가 N개가 있을 때, for문 같은 걸로 한 번만 돌리면 끝나는 것, 
    - for문 한 번만 돌린다는 의미는 for문을 돌리면서 데이터를 배열에 하나씩 넣어놓고 나중에 그걸 처리하나, 
    - 아니면 데이터가 들어오는 족족 처리하나 같은 것이다. 
    - 그래서 굳이 저장할 필요 없고 한 번에 한 글자씩 읽어 오면서 처리할 수 있어 좋다.
    - 이렇게 처리하는게 메모리 적게 먹고 복잡하게 코드 작성 안 할수 있는 방법이다.
    - 큰 문제를 굉장히 자잘하게 나눠서 똑같은 단계를 모든 데이터에 반복하며 돌게 만드는 그 알고리듬을 생각하는 습관을 길러야 한다.

### 한 줄씩 읽기 - fgets()로 안전하게 한 줄 읽기
```c
char* fgets(char* str, int count, FILE* stream);
```
- 최대 count - 1 개의 문자열을 읽어서 str에 저장
- 즉, 새 줄을 만나지 않아도 이 함수가 반환될 수 있음!
- str에 새 줄 문자(\n)까지 넣어줌
    - 새 줄을 만나서 끝났을 때랑 아닐 때를 구분해야 하기 때문에
- 매개 변수
    - str : gets()와 마찬가지로 입력받은 한 줄을 저장할 char 배열(역시 const가 아님)
    - count : 널 문자를 포함하기 때문에 실제로 읽어오는 문자 수는 count - 1 개
    - stream : 데이터를 읽어올 스트림; 키보드 입력을 읽어오고 싶으면 stdin을 넣어 주면 된다!
- 반환 값
    - 성공 시, str
    - 실패 시, NULL
- gets() - 절대 사용금지!! 매우매우 위험한 함수; C11는 아예 함수를 제거함
    
### 한 줄씩 읽는 알고리
1. 한 줄을 읽어온다
2. 한 줄을 읽어오는데 실패하면 프로그램을 종료
3. 성공했다면 한 줄 읽어온 데이터를 필요에 따라 사용
4. 1번 단계로 되돌아 간다.

### 한 줄을 어떻게 읽지?
- C는 절대 그렇게 돌지 않는다.
- 프로그래머가 미리 메모리 할당해서 만든 배열을 함수에 전달
- 함수는 그 배열에 한 줄을 읽어옴
- 즉, 한 줄을 읽어오면 함수가 새로운 문자열을 반환해주는 것이 아니다.
```c
#include <stdio.h>

#define LINE_LENGTH (10)

char line[LINE_LENGTH];

while (fgets(line, LINE_LENGTH, stdin) != NULL)
{
    printf("%s",line);
}
```
- 코드 읽기
    - stdin : `123456789abcd\n`
    - line : `|1|2|3|4|5|6|7|8|9|\0|`
        - 성공적으로 읽었으므로 NULL이 아님!
    - line을 출력 : 123456789
        - 루프 반복        
    - stdin : 123456789`abcd\n`
        - 아직 입력 스트림 stdin에 남아 있는 문자들을 읽음
    - line : `|a|b|c|d|\n|\0`|7|8|9|\0|
        - 읽는 중간에 새 줄 문자(\n)를 만남
        - 새 줄 문자까지의 문자열을 line에 넣음
        - 언제나 배열의 크기는 충분히 크게 잡을 것        
        - 성공적으로 읽었으므로 NULL이 아님!
    - line을 출력 : 123456789abcd
    - stdin ^z
        - EOF를 넣어 줬으므로 NULL이 반환
        
### 한 줄씩 읽는 방법이 유용한 경우
- 일단 단어 하나씩 읽는 것보단 한 줄씩 읽는게 빠름
- CPU를 벗어나 외부 구성요소로부터 뭔가를 읽어올 때는 한 번에 많이 읽어오는게 빠르기 때문
- 따라서 버퍼 크기는 충분히 큰게 좋다
    
### 한 데이터씩 읽는게 무슨 의미일까?
- 여태까지 읽는 것들 한 글자씩, 아니면 한 줄을 문자열로 읽었지만 
- 이거는 그게 아니라 텍스트로 들어오는 내용이긴 한데, ‘얘는 숫자로 읽고 싶어, 
- 얘는 정수, 얘는 실수로 읽고 싶어’ ‘아니면 얘는 문자열로 읽고 싶어, 얘는 한 글자만 읽을게’ 
- 이런식으로 **내가 원하는 데이터 형,**
- **즉, 어떤 데이터 형을 읽을지 지정해줘서 읽어 올 수 있다는 것(입력을 받을 수 있다는 것),** 
- **그게 한 데이터씩 읽기이다.**
- 출력의 printf()라는 함수, 즉 한 데이터씩 읽기에 대응하는 입력 함수라고 생각하면 된다.
- 한 데이터씩 읽어올 때 쓸 일이 더 많다 (안전하게 사용하기 위해)

### 한 데이터씩 읽기 - scanf(), fscanf(), sscanf()
```c
int scanf(const char* format, ...);
int fscanf(FILE* stream, const char* format, ...);
int sscanf(const char* buffer, const char* format, ...);
```
- **scanf()**    
    - 키보드(stdin)로부터 입력을 받아 변수에 저장
    - `scanf("%d", &num);`
        - 참조에 의한 전달 흉내 중
        - 그냥 num을 넣으면 복사된 매개변수(값에 의한 전달)
        - 함수 속에서 바꿔봐야 반환 시 사라지게 된다.
    - 반환값
        -   몇 개의 데이터를 읽었는지 반환(한 개를 읽으면 1을 반환)
        -   첫 데이터를 읽기 전에 실패했다면 EOF를 반환
- **fscanf()**
    - 파일 스트림으로부터 읽음
- **sscanf()**
    - C-Style 문자열로부터 읽음
    
### scanf()는 문자열 읽을 때 쓰면 별로임
- 숫자만 읽어야 하는데 문자를 읽으면...
- 그 뿐만 아니라 다른 자료형 읽을 때도 툭하면 무한 루프에 빠질 위험도 큼
- **해결법?**
    - 앞에 배웠던 fgets()와 sscanf()함수를 같이 쓰는 게 좋음
    - fgets()로 읽어온 어떤 string 버퍼를
    - sscanf()로 사용해서 거기서 읽어온다.
```c
#include <stdio.h>

#define LINE_LENGTH (1024)

int sum = 0;
int num;
char line[LINE_LENGTH];

// 입력 스트림(stdin): 10a\n
while (TRUE)
{     
    if (fgets(line, LINE_LENGTH, stdin) == NULL)  // line : 10a\n\0
    {
        clearerr(stdin);
        break;
    }

    if (sscanf(line, "%d", &num) == 1) // line에서 숫자만 읽어옴
    {                                  // 한 개 읽었으므로 통과!
                                       // num에 10 입력
        sum += num;
    }
}            
```

### 버퍼 오버플로 문제 없이 문자열 읽기
- **문자열 읽기**에서 이 방법으로 아주 기본적으로
- 많이 사용하니 익숙해지도록 익혀두자!!
```c
#include <stdio.h>

#define LENGTH (4096)

// 문자열을 읽을 때도 버퍼 오버플로를 막기위해
// word에서 읽어올때 이 word 길이를 한 줄 읽어오는 길이랑 일치시켜버린다.
// 그래서 4096보다 긴 문자열이 들어오면 그냥 짤리는게 전부이다.
// fgets()가 최대 LENGTH(4096) char 읽어 오는데 
// 그거를 다시 4096 char 길이만큼 char 배열에 읽는다고 해서 문제가 생길 수 없다.

char line[LENGTH];
char word[LENGTH];

// 입력 스트림(stdin): 10a\n
while (TRUE)
{     
    if (fgets(line, LENGTH, stdin) == NULL)  // line : 문자열 길이만큼\n\0
    {
        clearerr(stdin);  // 오류 표시자 지워주는 함수 
        break;
    }

    if (sscanf(line, "%s", word) == 1) // line에서 읽어와서 word에 문자열 입력
    {
        printf("%s\n", word);
    }
}            
```

### 한 데이터씩 읽는 방법이 유용한 경우
- 텍스트를 다른 자료형으로 곧바로 읽어오는 가장 간단한 방법
- 사용자 입력 받을 때(그리고 여러 데이터가 혼용된 텍스트 파일을 읽어올 때) 가장 많이 쓰는 방법

### 한 블록 읽기 - fread()
```c
size_t fread(void* buffer, size_t size, size_t count, FILE* stream);
```
- size 바이트짜리 데이터를 총 count개수만큼 읽음
- 그래서 buffer에 저장
- EOF 만나면 당연히 멈춤
- 실제로 읽은 개수를 반환
```c
int nums[64];      // int 블록 총 64개 
size_t num_read;   // 총 몇 바이트? 64 * sizeof(int)
FILE* fstream;

num_read = fread(nums, sizeof(nums[0]), 64, fstream);
fwrite(nums, sizeof(nums[0]), 64, fstream);
```

### 한 블록씩 읽는 방법이 유용한 경우
- 가장 중요한 건 이진 데이터 읽기 위해
- 이진 데이터를 하나씩 읽을 수 있지만 한꺼번에 읽으면 성능 향상

### 한 블록씩 읽을 때 주의할 점
- 기본 데이터형의 크기는 시스템마다 다름
- 따라서 이런 일을 하려면 정확히 파일에 저장할 데이터 크기를 고정해 두는게 좋음

# 파일 입출력
- C에서 파일 관련 연산은 다 이렇다.
    - 파일을 열어서 파일 스트림을 가져온다
    - 그 파일 스트림을 사용해서 하고 싶은 걸 한다
    - 그 파일을 닫아 준다

### 파일 열기 - fopen()
```c
FILE* fopen(const char* filename, const char* mode);
```
- filename으로 지정된 파일을 연다.
- 열 때 사용하는 모드(읽기전용, 이진파일 등)는 mode로 지정
    - r : 파일을 읽기 전용
    - w : 파일을 쓰기 전용
    - a : 파일에 이어 쓴다
    - b를 붙이면 이진 모드로 파일을 연다.
        - rb, wb, ab, r+b, w+b, a+b
    - 이진 모드란
        - 사실 유닉스 계열에는 아무 차이가 없다 (rb나 r이나 동일)
        - 윈도우에서 새 줄 문자 처리하는 것만 달라짐
            - 프로그램에서 **텍스트 모드**로 열었을 때 : `|H|e|l|l|o|\n|`
            - 프로그램에서 **이진 모드**로 열었을 때 : `|H|e|l|l|o|\r|\n|`
- 반환값은 파일 스트림 포인터!
```c
#include <stdio.h>

#define LENGTH (1024)

FILE* stream;
char list[LENGTH];

stream = fopen("hello.txt", "r");

if (fgets(list, LENGTH, stream) != NULL)
{
    printf("%s", list);
}
```

### 파일에 쓰기 - fwrite()
```c
size_t fwrite(const void* buffer, size_t size, size_t count, FILE* stream);
```
- `const void* buffer`
    - 첫 번째 인자로 buffer가 char*도 아니고 int*도 아니고 float*도 아닌 그냥 void*임
    - 즉 fwrite() 입장에서는 그냥 비트패턴이 줄줄이 들어온다
        - char*로 들어오면 1바이트 단위로 읽어서 아스키코드로 인식하지만
        - void*기 때문에 숫자에 의미가 없어진다.
    - 0x0A가 fwrite() 입장에서는 '\n'를 의미하는 건지 정수 '10'을 의미하는 건지 아니면 부동소수점의 일부인지 알 수가 없음
    - 따라서 fflush() 만이 유일한 해결
- 바로 파일에 쓰려면? fflush() 호출
```c
FILE* stream;
int scores[LENGTH] = { 20, 73, 33, 44, 55 };

stream = fopen(filename, "wb");

fwrite(scores, sizeof(scores[0]), LENGTH, stream);
fflush(stream);
```

### 파일 읽기
```c
#include <stdio.h>
#include <string.h>

#define LENGTH (6)

void read_file(const char* filenmae)
{
    FILE* stream;
    char data[LENGTH];
    
    stream = fopen(filename, "rb");
    
    while (TRUE)
    {
        if (fgets(data, LENGTH, stream) == NULL)
        {
            break;
        }
        printf("%s\n", data);
    }
}
```
```c
#include <stdio.h>
#include <string.h>

#define LENGTH (1024)

void append_file(const char* filenmae)
{
    FILE* stream;
    char data[LENGTH];
    
    stream = fopen(filename, "ab");   

    if (fgets(data, LENGTH, stdin) != NULL)
    {
        fwrite(data, strlen(data), 1, stream);
    }    
}

append_file("hello.txt");
```
```c
#include <stdio.h>
#include <string.h>

#define LENGTH (1024)

void open_file(const char* filenmae)
{
    FILE* stream;
    char data[LENGTH];
    
    stream = fopen(filename, "rb");   
    if (stream == NULL)  // fopen()는 실패하면 NULL 포인터를 반환
    {                    // 실패 시 오류 메시지 출력
        fprintf(stderr, "error while opening %s", filename);
        
        // 오류메시지 처리해주는 함수 perror() 사용 예시:
        perror("error while opening"); 
        
        return;        
    }
    
    if (fgets(data, LENGTH, stream) != NULL)
    {
        printf("%s", data);
    }
    
    if (fclose(stream) != 0)  // 파일을 닫음, 성공하면 0, 실패하면 EOF
    {
        fprintf(stderr, "error while closing"); // 실패 시 오류 메시지 출력
    }
}

int main(void)
{
    open_file("hello.txt");
    
    return 0;
}
```

# 스트림의 위치
- EOF 표시자
- 오류 표지사
- 파일 위치 표시자

### 스트림 위치를 시작(처음)으로 돌리기
```c
void rewind(FILE* stream);
```

### 스트림 위치를 임의의 위치로 옮기기
```c
int fseek(FILE* stream, long offset, int origin);
```
- 파일 위치 표시자를 origin으로부터 offset만큼 이동
- 위치 이동에 성공하면 0을 반환
- 실패하면 0이 아닌 수를 반환
- origin
    - SEEK_SET : 파일의 시작
    - SEEK_CUR : 현재 파일의 위치
    - SEEK_END : 파일의 끝
- offset
    - 어떤 기준점으로부터 얼마만큼 떨어져있나 표현
    - 기준점(origin)부터 3바이트 뒤는 오프셋 3
    - 기준점부터 3바이트 전은 오프셋 -3


### 파일 위치 표시자의 현재 위치를 알고 싶다면
```c
long ftell(FILE* stream);
```
- 파일 위치 표시자의 현재 위치를 알려주는 함수
- 실패하면 -1 반환 


# 입출력 리디렉션(IO redirection)
- 입출력 리디렉션은 C의 기능이 아니라 커맨드 라인 또는 shell의 기능이다.
- 입력이나 출력이 들어오고 나가는 방향을 다른 데로 돌려줌
- 입력 리디렉션
    - 텍스트 파일을 열어 stdin에 대신 타이핑 쳐주는 기능
    - `stdin: <`
- 출력 리디렉션
    - stdout 또는 stderr에 출력되는 것을 화면에 보여주는 대신 텍스트 파일에 저장
    - `stout: >`
    - `stderr: 2>` (두 번째 출력)
- ` > a.exe < input.txt > output.txt 2> error.txt `

- 빈 input.txt를 읽을 때
```c
// 코드 생략

if (fgets(filename, FILE_LENGTH, stdin) == NULL)
{
    fprintf(stderr, "no input\n");
    return;
}

// 코드 생략
```
- ` > a.exe < input.txt > output.txt 2> error.txt `
    - input.txt : 빈 데이타
    - output.txt : 빈 데이타
    - error.txt : no input
    
- input.txt의 숫자를 출력하기
```c
int score;

while (scanf("%d", &score) == 1)
{
    printf("%d ", score);
}
```
- ` > a.exe < input.txt > output.txt 2> error.txt `
    - input.txt : 65 57 53 34 32 36 23
    - output.txt : 65 57 53 34 32 36 23
    - error.txt : 빈 데이터

- 타이핑 안치고 파일로 stdin으로 전달 가능!
- 이렇게 stderr하고 stdout가 분리 가능!


### 커맨드 라인에서 프로그램 실행할 때 인자들을 넣어주는 방법
- 매개변수를 안 받는 메인 함수
```c
int main(void)
{
}
```
- 매개변수를 받는 메인 함수
```c
int main(int argc, const char* argv[])
{
}
```
- argc는 들어온 인자의 수
    - 이 수는 실행한 팡리의 이름까지 포함
    - `> fliecopy.exe a.txt b2.txt`
    - 위의 경우 argc는 3이다.
- argv는 char 포인터 배열
    - argv[argc + 1]로 생성된다.
    - 커맨드 라인 인자들의 시작 주소들을 argv[] 배열에 넣는다.
    - argv[0] : 첫 번째 요소에서 실행 파일의 이름
    - argv[1] ~ argv[argc-1] :  커맨드 라인 인자들이 순차적으로 들어옴
    - argv[argc] : NULL
        

