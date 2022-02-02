### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 입력(Input)
- std::cin
    - get()
    - getline()
    - eof()
    - fail()
    - clear()
    - ignore()

- 키보드에서 안전하게 읽는 법
    - C
    ```js
    char line[512];
    char temp[512];
    char firstName[4];

    if (fgets(line, 512, stdin) != NULL)
    {
        if (sscanf(line, "%s", temp) == 1 && strlen(tem) < 4)
        {
            strcpy(firstName, temp);
        }
    }
    ```
    - C++