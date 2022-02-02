### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 입력(Input)
- std::cin
    - 스트림 읽어오기
        - get()
            - `\n`(new line) 문자를 만나기 직전까지의 모은 문자를 가져온다.
            - new line 문자는 입력 스트림에 남아 있음
            - get(firstName, 100)
                - 99개 문자 + 널캐릭터 = 100
                - 99개 문자를 가져오거나 뉴라인 문자가 나올 때까지의 문자를 가져오고
                - 가져온 문자들을 firstname(char 배열)에 배치한다.
                - 뉴라인 문자는 입력 스트림에 남아 있음
        - getline()
            - 뉴라인 문자를 만기 직전까지의 모든 몬자를 가져온다.
            - 뉴라인 문자는 입력 스트림에서 버림
    - 스트림 상태 나타내기
        - eof()
        - fail()
    - 스트림 입력 버리기
        - clear()
        - ignore()
            - cin.ignore(10);   `// 문자 10개를 버림`

- 키보드에서 안전하게 읽기
    - C
    ```C
    char line[512];    // char[] == char*
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
    int number;
    int sum = 0;

    while (true)
    {
        cout << "Please enter an integer or EOF: ";
        cin >> number;    // int 타입만 읽음
        if (cin.eof())    // 스트림 상태을 읽음
        {
            break;
        }

        if (cin.fail())  // 스트림 상태를 읽음
        {
            cout << "Invalid input" << endl;
            cin.clear();  // 스트림 데이터 버림
            cin.ignore(LLONG_MAX, '\n');  // 스트림 데이터 버림
            continue;
        }
        sum += number;
    }
    cin.clear();  // 스트림 데이터 버림

    cout << "The sum is " << sum << endl;
    ```

    ```C++
    const int LINE_SIZE = 512;
    char line[LINE_SIZE];

    cout << "Please enter a string to reverse" << endl
        << "or EOF to quit: ";

    cin.getline(line, LINE_SIZE);  // 스트림 데이터 가져오기. LINE_SIZE 만큼 읽어 line에 저장
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