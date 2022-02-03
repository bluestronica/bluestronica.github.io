### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 파일 입출력(I/O)
- #### `<fstream>`
    - 읽기 전용으로 파일 오픈
    ```c++
        ifstream fin;
        fin.open("helloWorld.txt");
    ```
    - 쓰기 전용으로 파일 오픈
    ```c++
        ofstream fout;
        fout.open("helloWorld.txt");
    ```
    - 읽기, 쓰기 범용으로 파일 오픈
    ```c++
        fstream fs;
        fs.open("helloWorld.txt");
    ```