### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 파일시스템(Filesystem), 모듈(Module)시스템
- #### 파일시스템(Filesystem)
    - C++17의 새로운 라이브러리
    - C++14나 그 전에는 파일 시스템과 다음과 같은 구성요소에 대해 연산을 할 방법이 없었음
        - 경로(path)
        - 일반팡리
        - 디렉터리(directory)
    - 파일 읽기와 쓰기에 관한 라이브러지가 아님
    - 파일 속성 변경, 디렉터리 순회, 파일 복사 등에 관한 라이브러리
    - 이 모든 걸 std::fs로 할 수 있음
    - https://en.cppreference.com/w/cpp/filesystem

- #### 파일시스템 연산
    - 플랫폼 공통적인 방법으로 경로 합치기
    - 파일과 디렉터리를 복사, 이름 바꾸기, 삭제
    - 디렉터리에서 파일, 디렉터리 목록 가져오기
    - 파일 권한 읽기 및 설정
    - 파일 상태 읽기 및 설정