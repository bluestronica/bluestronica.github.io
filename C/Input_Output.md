[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 출력 (Output)

### 서식 지정(formatted) 출력
- **C에서 출력을 논할 때 가장 기본이 되는 함수**
    - **`printf()`** : 콘솔창(stdout)에 출력
        ```c
        const char* msg = "Hello World";

        printf("%s\n", msg);
        printf("Hello World)";
        ```
    - **`fprintf()`** : 스트림에 출력
        ```c
        const char* msg = "Hello World";

        fprintf(stdout, "%s\n", msg);
        fprintf(stdout, "Hello World\n");
        fprintf(stderr, "Error!!\n");
        ```
    - **`sprintf()`** : 문자열에 출력
        - 출력은 char 배열에!!
        - 정말 많이 씀!
        - 심지어 C++에서 string 클래스가 있는데도 이걸 대신 많이 씀
        - 쓰는 이유는 속도 때문(가장 빨리 문자열을 조작하는 함수는 C함수)
        - 다만, 프로그래머가 충분히 큰 버퍼를 잡아주지 않으면 위험!
        ```c
        char buffer[100];
        int score = 100;
        const char* name = "Hello";

        sprintf(buffer, "%s: %d", name, score);

        printf("%s\n", buffer);
        ```
- **서식 문자열이 필요한 이유**
    - 서식 문자열은 추가 메모리 할당 없이 있는 자료형을 출력 스트림에 문자들로 출력해줌!

- **보통 프로그램이 실행할 때 기본적으로 3개의 스트림을 준다.**
    - **`stdout`** (콘솔 출력)
        - 콘솔 스트림
        - 보통 라이 버퍼링(line buffering)을 사용
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
    - **문자열**을 **stdout**에 출력
    - 마지막에 줄도 바꿔줌: `\n`
    - **`fputs(str, stdout)`**와 매우 비슷

- **putchr()**
    - **문자**를 **stdout**에 출력
    - **`fputc(ch, stdout)`**하고 같음
