### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 파일 입출력(I/O)
- #### `<fstream>`
    - 읽기 전용으로 파일 오픈
        ```C++
        ifstream fin;
        fin.open("helloWorld.txt");  // txt 파일을 fin에 저장
        ```
    - 쓰기 전용으로 파일 오픈(파일 없으면 만든다.)
        ```C++
        ofstream fout;
        fout.open("helloWorld.txt");  // fout을 txt 파일에 저장
        ```
    - 읽기, 쓰기 범용으로 파일 오픈
        ```C++
        fstream fs;
        fs.open("helloWorld.txt");  // 쓰기, 읽기 범용
        ```

- ### 파일 읽기
    - 파일에서 문자 하나씩 읽기
        ```C++
        ifstream fin;
        fin.open("helloWorld.txt");

        char character;
        while (true)
        {
            fin.get(character);
            if (fin.fail())
            {
                break;
            }
            cout << character;
        }

        fin.close();
        ```
    - 파일에서 한 줄씩 읽기
        ```C++
        ifstream fin;
        fin.open("helloWorld.txt");

        string line;
        while (!fin.eof())
        {
            getline(fine, line);
            cout << line << endl;
        }

        fin.close();
        ```
    - 파일에서 한 단어씩 읽기
        ```C++
        ifstream fin;
        fin.open("helloWorld.txt");

        string name;
        float balance;
        while (!fine.eof())
        {
            fin >> name >> balance;
            cout << name << ": $" << balance << endl;
        }

        fin.close();
        ```
    - get(), getline(), >> (밀어넣기 연산자)
        - 어떤 스트림(예: cin, istringstream)을 넣어도 동일하게 동장
            ```C++
            fin.get(character);

            fin.getline(firstName, 20);  // 파일에서 문자 20개를 읽음
            getline(fin, line);  // 파일에서 한 줄을 읽음. \n 뉴라인 만날 때까지

            fin >> word;  // 파일에서 한 단어를 읽음
            ```
    - EOF 처리는 까다롭다.
        - 입출력 연산이 스트림 상태 비트를 변경한다는 사실을 기억할 것
        - EOF를 잘못 처리하면 무한 반복을 초래
        - clear()를 쓸 때는 두 번 생각하자
    - 훌륭한 테스트 케이스
        - 유효한 입력 뒤에 EOF
        - 유효하지 않은 입력 뒤에 EOF
        - 유효하지 않은 입력과 뉴라인(\n)뒤에 EOF
        - 공백: 탭도 체크
        - 키보드 입력과 입력 리다이렉션(redirection) 둘 다 확인할 것
            - 100 200 300
            - cmd> numbers.exe < numberinput.txt

- #### 파일에 쓰기
    - 파일에 문자 하나씩 써 넣기
        ```C++
        ofstream fout;
        fout.open("HelloWorld.txt");

        char character;
        get(cin, character);
        while (true)
        {            
            if (cin.fail())
            {
                break;
            }
            fout.put(character);
        }

        fin.close();
        ```
    - 파일에 한 줄씩 써 넣기
        ```C++
        ofstream fout;
        fout.open("HelloWorld.txt");

        string line;
        getline(cin, line);
        if (!cin.fail())
        {
            fout << line << endl;  // 'n\' 뉴라인을 쓸 경우 플러쉬가 일어나지 않고 endl은 플러쉬가 일어난다. 
        }

        fout.close();
        ```

- #### 바이너리 파일 읽기
    ```C++
    ifstream fin("studentRecords.dat", ios_base::in | ios_base::binary);

    if (fin.is_open())
    {
        Record record;
        fin.read((char*)&record, sizeof(Record));
        
        // 스택에 record 44바이트 공간이 잡힌다.
        // (char*)&record : record 시작주소(1024)를 나타내는 &record를 char 포인터로 캐스팅
        // &record 시작주소는 1024, Record의 사이즈는 44바이트
        // read()함수에 1024와 44바이트를 넘겨주면
        // 주소 1024부터 바이트 44개를 넣어라는 뜻이다.
    }

    fin.close();
    ```

- #### 바이너리 파일에 쓰기
    ```C++
    ofstream fout("studentRecords.dat", ios_base::out | ios_base::binary);

    if (fout.is_open())
    {
        char buffer[20] = "Blues tronica";
        fout.write(buffer, 20);
    }

    fout.close();
    ```

- #### 파일 안에서의 탐색
    ```C++
    fstream fs("helloWorld.dat", ios_base::in | ios_base::out | ios_base::binary);

    if (fs.is_open())
    {
        fs.seekp(20, ios_base::beg);
        if (!fs.fail())
        {
            // 21번째 위치에서부터 덮어쓰기
        }
    }

    fs.close();
    ```

- #### 탐색(seek) 유형
    - 절대적
        - 특정한 위치로 이동
        - 보통 tellp() / tellg()를 사용해서 기억해 놨던 위치로 돌아갈 때 사용
    - 상대적
        - ios_base::beg
        - ios_base::cur
        - ios_base::end
    - 파일 쓰기의 위치 읽기 및 변경
        - tellp()
            - 쓰기 포인터의 위치를 구함
            - ios::pos_type pos = fout.tellp();
        - seekp()
            - 절대적
                - fout.seekp(0);
                - 처음 위치로 이동
            - 상대적
                - fout.seekp(20, ios_base::cur);
                - 현재 위치로부터 20바이트 뒤로 이동
    - 파일 읽기의 위치 읽기 및 변경
        - tellg()
            - 읽기 포인터의 위치를 구함
            - ios::pos_type pos = fin.tellg();
        - seekg()
            - 절대적
                - fin.seekg(0);
                - 처음 위치로 이동
            - 상대적
                - fin.seekg(-10, iso_base::end);
                - 파일의 끝에서부터 10 바이트 앞으로 이동
- #### 기타 정보
    - `>>`를 getline()과 같이 쓰지 말 것
        ```C++
        cin  >> number;
        getline(cin, line);   // cin에서 공백을 꺼내 감
        ```
    - 해결법
        ```C++
        cin >> number;
        cin >> std::ws;       // 공백을 버림
        getline(cin, line); 
        ```