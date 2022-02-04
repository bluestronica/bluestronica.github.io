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
    - get(), getline(), >>
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
            fout << line << endl;
        }

        fout.close();
        ```
- 바이너리 파일 읽기
    ```C++
    ifstream fin("studentRecords.dat", ios_base::in | ios_base::binary);

    if (fin.is_open())
    {
        Record record;
        fin.read((char*)&record, sizeof(Record));

        // (char*)&record : record 시작주소(1024)를 나타내는 &record를 char 포인터로 캐스팅
        // 즉, char 포인터는 가리키는 주소(1024)에 가서 char(1바이트, 8비트) 크기 만큼 읽어 오겠다는 의미이다.
        // 1024 주소가 가리키는 곳에 가서 char 크기 1바이트 씩 읽겠다는 의미..
        // record 크기 만큼 읽는 것이 아니라 char 크기 1바이트 씩
    }

    fin.close();
    ```