### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 문자열(string)
- #### std::string 클래스
    - 대입(Assignment)
    - 덧붙이기(Appending)
    - 합치기(Concatenation)
    - 비교(Relational)연산자
    - size(), length()
    - c_str()
        - const char*
        - 해당 string이 가지고 있는 문자 배열의 시작 주소를 가리키는 포인터를 반환
        ```C++
        string line;
        cin >> line;
        const char* cLine = line.c_str();
        ```
    - string 속의 한 문자의 접근은 C와 같음
    ```C++
    string firstName = "blues";
    char letter = firstName[1];

    firstName[2] = 'P';
    ```
    - `\n` 문자를 만날 때까지 `스트림`에서 문자들을 꺼내서 `string`에 저장
        - getline()
            - 다음의 조건을 만족할 때까지 계속해서 스트림에서 문자들을 꺼내 string에 저장
                - end-of-file을 만날 때(eofbit 값이 true가 됨)
                - 구분 문자(delimiter)를 만날 때까지 (구분 문자는 버려짐!)
        ```C++
        string mailHeader;
        getline(cin, mailHeader); // `\n`문자를 만날 때까지 cin에서 문자들을 꺼내서 mailHeader에 저장

        getline(cin, mailHeader, '@'); // '@' 문자를 만날 때까지 cin에서 문자들을 꺼내서 mailHeader에 저장
        ```

- #### `<sstream>`
    - std::istringstream
        - 키보드 대신 string으로부터 읽어옴
        - cin과 비슷
        - sscanf()와 비슷
    - std::ostringstream        
        - 콘솔 대신 string에 출력
        - cout와 비슷
        - sprintf()와 비슷
    - 허나, 그렇게 자주 쓰이지는 않음

- #### 여전히 성능상의 이유로 많은 C함수들을 사용
    - `<cstring>`
    - `<cstdio>`
    - `<cctype>`
    - 힙(heap) 메모리 할당은 느림
    - 메모리 단편화 문제
    - 내부 버퍼의 증가는 멀티 쓰레드 환경에서 안전하지 않을 수도        
    - 그래서, 여전히 `sprintf()`와 함께 `char[]`를 매우 많이 사용!!!!  
        - int sprintf(char* buffer, const char* format, ...);
        - 출력은 char 배열에!
        - C++에서 string 클래스가 있는데도 이걸 대신 많이 쓴다.
        - 쓰는 이유? 속도 때문(가장 빨리 문자열을 조작하는 함수는 C함수)
        - 다만, 프로그래머가 충분히 큰 버퍼를 잡아주지 않으면 위험!!!
        ```C++
        char buffer[100];
        int score = 100;
        const char* name = "Rachel";

        sprintf(buffer, "%s: %d", name, score);

        cout << buffer << endl;
        ```

- #### Samples
    ```C++
    string line = "Hello world!";

    cout << "string to mirror: " << line << endl;

    for  (int i = (int)line.size() - 1; i >= 0; --i)
    {
        line += line[i];
    }
    cout << "mirrored string " << line << endl;
    ```

    ```C++
    string firstName;
    string lastName;
    string studentID;
    int score;

    istringstream inputStream("blues tronica A12345678 70");
    ostringstream outputStream;

    inputStream >> firstName >> lastName >> studentID >> score;
    outputStream << firstName << " " << lastName << " " << studentID << " " << score;

    cout << outputStream.str() << endl;
    ```