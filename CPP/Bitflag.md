### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 비트플래그(Bitflag)
- #### 비트 플래그 정의
    - C++14는 바이너리 리터럴을 제공
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 0b0000'0001;  // represents bit 0 
        const unsigned char option1 = 0b0000'0010;  // represents bit 1 
        const unsigned char option2 = 0b0000'0100;  // represents bit 2 
        const unsigned char option3 = 0b0000'1000;  // represents bit 3 
        const unsigned char option4 = 0b0001'0000;  // represents bit 4 
        const unsigned char option5 = 0b0010'0000;  // represents bit 5 
        const unsigned char option6 = 0b0100'0000;  // represents bit 6 
        const unsigned char option7 = 0b1000'0000;  // represents bit 7
        ```
    - C++11 또는 이전 버전에서 비트 플래그 정의
        - 16진수를 사용하는 게 일반적인 방법
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 0x1;  // hex for 0000 0001 
        const unsigned char option1 = 0x2;  // hex for 0000 0010 
        const unsigned char option2 = 0x4;  // hex for 0000 0100 
        const unsigned char option3 = 0x8;  // hex for 0000 1000 
        const unsigned char option4 = 0x10; // hex for 0001 0000 
        const unsigned char option5 = 0x20; // hex for 0010 0000 
        const unsigned char option6 = 0x40; // hex for 0100 0000 
        const unsigned char option7 = 0x80; // hex for 1000 0000
        ```
        - 왼쪽 시프트 연산자(`<<`)를 사용한 더 쉬운 방법도 있다.
        ```c++
        // Define 8 separate bit flags (these can represent whatever you want) 
        const unsigned char option0 = 1 << 0;  // 0000 0001 
        const unsigned char option1 = 1 << 1;  // 0000 0010 
        const unsigned char option2 = 1 << 2;  // 0000 0100 
        const unsigned char option3 = 1 << 3;  // 0000 1000 
        const unsigned char option4 = 1 << 4;  // 0001 0000 
        const unsigned char option5 = 1 << 5;  // 0010 0000 
        const unsigned char option6 = 1 << 6;  // 0100 0000 
        const unsigned char option7 = 1 << 7;  // 1000 0000
        ```

- #### 비트 켜기 (Turning individual bits on)
    - 비트 OR 연산자(`|`)를 사용해 비트를 켤 수 있다.
        ```c++
        // 위에서 정의한 8가지 옵션을 위해 8비트를 사용
        unsigned char myflags = 0; // all bits turned off to start

        myflags |= option4;  // turn option4 on

        ///////////////////////
        myflags = 0000 0000  flag
        option4 = 0001 0000  mask
        -------------------   or
        result  = 0001 0000  flag
        ```
- #### 비트 끄기 (Turning individual bits off)
    - 비트 AND 연산자(`&`)와 비트 NOT 연산자(`~`)를 이용해서 비트를 끌 수 있다.
        ```c++
        myflags &= ~option4;  // turn option4 off
                              // myflags = (myflags & ~option4)

        //////////////////////////
        myflags   = 0001 0000  flag
        ~options4 = 1110 1111  ~mask
        ---------------------  and
        result    = 0000 0000  flag    


        //////////////////////////
        myflags   = 0001 1100  flag
        ~options4 = 1110 1111  ~mask
        ---------------------  and
        result    = 0000 1100  flag        

        // 여러 비트 동시에 끄기
        myflags &= ~(option4 | option5);                
        ```

- #### 비트 뒤집기 (Flipping individual bits)
    - 비트가 커져 있다면 끄고, 꺼져 있다면 켜는 방법을 토글(toggle)이라고 한다.
    - 비트 XOR 연산자(`^`)를 이용해서 비트를 토글(toggle)할 수 있다.    
    - 두 비트가 다르면 1, 같으면 0
        ```c++
        myflags ^= option4; 

        /////////////////////
        myflags   = 0000 1100  flag
        options4  = 0001 0000  mask
        ---------------------  XOR
        result    = 0001 1100  flag 

        myflags ^= (option4 | option5);
        ```

- #### 비트가 커져 있는지 확인 (Determining if a bit is on or off)
    - 비트 AND 연산자(`&`)를 이용해서 비트의 상태를 알 수 있다.
        ```c++
        if(myflags & option4)
            std::cout << "myflags has option4 set";
        else
            std::cout << "myflags does not have option4 set";
        ```

- #### 32비트 16진수 값을 입력 받아서 R, G, B 및 A의 8비트 색 값을 추출
    - 2^4 = 16, 2^8 = 256
    - 0000 = 16가지 표현 = F = 4비트
    - 0000 0000 = 256가지 표현 = FF = 8비트 = 1바이트
        - 경우의 수는 256개
        - 고민은 0부터 255까지
    - 0000 0000 0000 0000 = 42.9억가지 표현 = FFFF = 32비트 = 4바이트
    ```c++
    #include <iostream> 
    int main() 
    { 
        const unsigned int redBits = 0xFF000000; 
        const unsigned int greenBits = 0x00FF0000; 
        const unsigned int blueBits = 0x0000FF00; 
        const unsigned int alphaBits = 0x000000FF; 
        std::cout << "Enter a 32-bit RGBA color value in hexadecimal (e.g. FF7F3300): "; 
        unsigned int pixel; std::cin >> std::hex >> pixel; // std::hex allows us to read in a hex value 
        
        // use bitwise AND to isolate red pixels, then right shift the value into the range 0-255 
        unsigned char red = (pixel & redBits) >> 24; 
        unsigned char green = (pixel & greenBits) >> 16; 
        unsigned char blue = (pixel & blueBits) >> 8; 
        unsigned char alpha = pixel & alphaBits; 

        std::cout << "Your color contains:\n"; 
        std::cout << static_cast<int>(red) << " of 255 red\n"; 
        std::cout << static_cast<int>(green) << " of 255 green\n"; 
        std::cout << static_cast<int>(blue) << " of 255 blue\n"; 
        std::cout << static_cast<int>(alpha) << " of 255 alpha\n"; 
        return 0; 
    } 
    
    //Enter a 32-bit RGBA color value in hexadecimal (e.g. FF7F3300): 
    //FF7F3300 Your color contains: 
    //255 of 255 red 
    //127 of 255 green 
    //51 of 255 blue 
    //0 of 255 alpha
    ```