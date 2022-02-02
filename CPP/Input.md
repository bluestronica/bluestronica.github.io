### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 입력(Input)
- std::cin
    - 스트림 읽어오기
        - get()
        - getline()
    - 스트림 상태 나타내기
        - eof()
        - fail()
    - 스트림 입력 버리기
        - clear()
        - ignore()
            - cin.ignore(10);   `// 문자 10개를 버림`

- 키보드에서 안전하게 읽는 법
    - C
    ```C++
    char line[512];
    char temp[512];
    char firstName[4];

    if (fgets(line, 512, stdin) != NULL)    // 스트림 stdin 상태가 NULL 아닐 때까지
    {
        if (sscanf(line, "%s", temp) == 1 && strlen(temp) < 4)
        {
            strcpy(firstName, temp);
        }
    }
    ```
    - C++
    ```C++
    const int LINE_SIZE = 512;
    char line[LINE_SIZE];

    cout << "Please enter a string to reverse" << endl
        << "or EOF to quit: ";

    cin.getline(line, LINE_SIZE);
    if (cin.fail())
    {
        cin.clear();
        return;
    }

    char* p = line;
    char* q = line + strlen(line) - 1;
    while (p < q)
    {
        char temp = *p;
        *p = *q;
        *q = temp;

        ++p;
        --q;
    }
    ```