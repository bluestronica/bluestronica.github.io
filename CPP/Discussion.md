### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

- ### (char*)&vector
    ```C++
    int main()
    {
        int i = 0x00020304;
        int* intPtr;
        char* charPtr;

        intPtr = &i;
        charPtr = (char*)intPtr;  // (char*)&i; 와 동치

        printf("정수형 i의 값 = %X\n", i);
        printf("정수형 i의 주소 = %p\n", &i);
        printf("정수형 포인터 intPtr의 주소 = %p\n", intPtr);
        printf("intPtr + 1 주소 = %p\n", intPtr + 1);  // 정수형 타입 포인터 + 1 은 4바이트씩 증가 
        printf("charPtr의 주소 = %p\n", charPtr);
        printf("charPtr + 1 주소 = %p\n", charPtr + 1);  // char 타입 포인터 + 1 은 1바이트씩 증가

        // 포인터 등가 수식에 대한 짧은 고찰
        printf("((char*)&i)[0] = %X\n", ((char*)&i)[0]);
        printf("*((char*)&i) = %X\n", *((char*)&i));

        printf("((char*)&i)[1] = %X\n", ((char*)&i)[1]);
        printf("*((char*)&i + 1) = %X\n", *((char*)&i + 1));

        if ( ((char*)&i)[0] )
            printf("Little Endian\n"); // Little Endian의 조건은 바이트 단위로 거꾸로 저장된다
                                    // [0x 04 03 02 00]
        else 
            printf("Big Endian\n");

        // 정리
        // 포인터가 가리키는 값에 대한 접근 범위는 타입에 따라 다르다.
        // int형 포인터로 값에 접근하면 4바이트씩 접근해서 값을 읽어온다.
        // char형 포인터로 값에 접근하면 1바이트씩 접근해서 값을 읽어온다.
        // 
        // int형 포인터로 접근한 i 값은 4바이트 크기의 [01020304]로 접근해서 값을 가져온다.
        // char형 포인터로 접근한 i 값은 1바이트 크기의 [01] [02] [03] [04] 접근해서 값을 가져온다.


        return 0;
    }
    ```