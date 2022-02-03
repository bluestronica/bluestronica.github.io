### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 문자열(string)
- #### std::string 클래스
    - 대입(Assignment)
    - 덧붙이기(Appending)
    - 합치기(Concatenation)
    - 비교(Relational)연산자
    - size(), length()
    - c_str()
    - string 속의 한 문자의 접근은 C와 같음
    - `\n` 문자를 만날 때까지 `스트림(cin)`에서 문자들을 꺼내서 string에 저장
        - getline()
    - sstream
        - 키보드 대신 string으로부터 읽어옴
        - 콘솔 대신 string에 출력
        - 허나, 그렇게 자주 쓰이지는 않음
            - 힙(heap) 메모리 할당은 느림
            - 메모리 단편화 문제
            - 내부 버퍼의 증가는 멀티 쓰레드 환경에서 안전하지 않을 수도
            - 여전히 성능상의 이유로 많은 C함수들을 사용
                - <cstring>
                - <cstdio>
                - <cctype>
            - 그래서, 여전히 `sprintf`와 함께 `char[]`를 매우 많이 사용!!!!
    ```C++

    ```