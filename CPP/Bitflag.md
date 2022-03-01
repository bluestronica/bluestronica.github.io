### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 비트플래그(Bitflag)
- #### 비트 플래그 정의
    - C++14는 바이너리 리터럴을 제공한다.
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 0b0000'0001; // represents bit 0 
        const unsigned char option1 = 0b0000'0010; // represents bit 1 
        const unsigned char option2 = 0b0000'0100; // represents bit 2 
        const unsigned char option3 = 0b0000'1000; // represents bit 3 
        const unsigned char option4 = 0b0001'0000; // represents bit 4 
        const unsigned char option5 = 0b0010'0000; // represents bit 5 
        const unsigned char option6 = 0b0100'0000; // represents bit 6 
        const unsigned char option7 = 0b1000'0000; // represents bit 7
        ```
    - C++11 또는 이전 버전에서 비트 플래그 정의
        - 16진수를 사용하는 게 일반적인 방법이다.
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 0x1; // hex for 0000 0001 
        const unsigned char option1 = 0x2; // hex for 0000 0010 
        const unsigned char option2 = 0x4; // hex for 0000 0100 
        const unsigned char option3 = 0x8; // hex for 0000 1000 
        const unsigned char option4 = 0x10; // hex for 0001 0000 
        const unsigned char option5 = 0x20; // hex for 0010 0000 
        const unsigned char option6 = 0x40; // hex for 0100 0000 
        const unsigned char option7 = 0x80; // hex for 1000 0000
        ```
        - 왼쪽 시프트 연산자(`<<`)를 사용한 더 쉬운 방법도 있다.
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 1 << 0; // 0000 0001 
        const unsigned char option1 = 1 << 1; // 0000 0010 
        const unsigned char option2 = 1 << 2; // 0000 0100 
        const unsigned char option3 = 1 << 3; // 0000 1000 
        const unsigned char option4 = 1 << 4; // 0001 0000 
        const unsigned char option5 = 1 << 5; // 0010 0000 
        const unsigned char option6 = 1 << 6; // 0100 0000 
        const unsigned char option7 = 1 << 7; // 1000 0000
        ```

- #### 비트 켜기 (Turning individual bits on)
    - 비트 OR 연산자 (`|`)를 사용해 비트를 켤 수 있다.
        ```c++
        // 위에서 정의한 8가지 옵션을 위해 8비트를 사용
        unsigned char myflags = 0; // all bits turned off to start

        myflags |= option4;  // turn option4 on

        ///////////////////////
        myflags = 0000 0000
        option4 = 0001 0000
        -------------------
        result  = 0001 0000
        ```
- #### 비트 끄기 (Turning individual bits off)
    - 비트 AND 연산자 (`&`)와 비트 NOT 연산자(`~`)를 이용해서 비트를 끌 수 있다.
        ```c++
        
        ```