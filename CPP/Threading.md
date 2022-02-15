### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 쓰레딩(Threading) 라이브러리
- #### 쓰레딩(Threading) 지원 라이브러리
    - 쓰레딩 라이브러리 사용법에 초점을 맞춤
        - 쓰레드
        - 뮤텍스(mutex)
        - 조건 변수(condition_variables)
        - 더 있음.
    - C++11 전까지 표준 멀티쓰레딩 라이브러리가 없었음
    - 그래서 OS마다 멀티쓰레딩 구현이 달랐음
        - 리눅스/유닉스 : POSIX 쓰레드(pthreads)
        - 윈도우 쓰레드

- std::thread
    - 표준 c++쓰레드
    - 이동(move) 가능
    - 복사 불가능