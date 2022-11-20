[bluestronica.github.io/C](https://bluestronica.github.io/C)
https://turtlog.tistory.com/8
https://www.programiz.com/c-programming/bitwise-operators

# Bitwise Operators In C


### 비트 연산자
- `& 연산자`
  - 둘 중 하나라도 0이면 결과가 0이 되는 특성이 있다.
  - 이 특성을 이용하면 **특정 비트를 0으로 변경**할 수 있다.
  ```
  12 = 00001100 (In Binary)
  25 = 00011001 (In Binary)

  Bit Operation of 12 and 25
    00001100
  & 00011001
    ________
    00001000  = 8 (In decimal)
  ```
  ```c
  int main(void)
  {
    int a = 12, b = 25;
    
    unsigned char data = 0xFF;  // 1111 1111
    unsigned char mask = 0x01;  // 0000 0001
    
    printf("Output = %d", a & b);       // Output = 8
    printf("Output = %d", data & mask); // 0000 0001 Output = 1
    
    return 0;
  }
  ```
- `| 연산자`
  - 둘 중 하나라도 1이면 결과가 1이 되는 특성이 있다.
  - 이 특성을 이용하면 **특정 비트를 1로 변경**할 수 있다.
  ```
  12 = 00001100 (In Binary)
  25 = 00011001 (In Binary)

  Bitwise OR Operation of 12 and 25
    00001100
  | 00011001
    ________
    00011101  = 29 (In decimal)
  ```
  ```c
  int main(void) 
  {
    int a = 12, b = 25;
    
    unsigned char data = 0x00;  // 0000 0000
    unsigned char mask = 0x01;  // 0000 0001
    
    printf("Output = %d", a | b); // Output = 29
    printf("Output = %d", data | mask); // 0000 0001 Output = 1

    return 0;
  }
  ```
- `^ 연산자`
  - 두 개가 같으면 0, 다르면 1인 특성이 있다.
  - 이 특성을 이용하여 **초기화, not 연산 구현, 암호화 및 복호화**가 가능하다.
  ```
  12 = 00001100 (In Binary)
  25 = 00011001 (In Binary)

  Bitwise XOR Operation of 12 and 25
    00001100
  ^ 00011001
    ________
    00010101  = 21 (In decimal)
  ```
  ```c
  int main(void) 
  {
    int a = 12, b = 25;
    
    printf("Output = %d", a ^ b); // Output = 21

    return 0;
  }
  ```
  ```c
  #include <stdio.h>
  #define DATA_SIZE 6
  
  int main(void)
  {
    char data[DATA_SIZE] = "Hello";  // DEC:72,101,108,108,111
    char key[DATA_SIZE] = "love ";   // DEC:108,111,118,101,32

    printf("befor encryption: %s\n", data);

    for(size_t i = 0; i < 6; ++i)
    {
      data[i] ^= key[i];
      // DEC:72  BIN 0100 1000
      // DEC:108 BIN 0110 1100
      // ^           0010 0100  DEC:36, ASIIC 코드 36은 `$'
    }
    printf("encryption: %s\n", data);

    for (size_t i = 0; i < 6; ++i)
    {
      data[i] ^= key[i];
      // DEC:36  BIN 0010 0100
      // DEC:108 BIN 0110 1100
      // ^           0100 1000  DEC:72, ASIIC 코드 72은 `H`
    }
    printf("decryption: %s\n", data);

    return 0;
  }
  ```
- `~ 연산자`
  - 모든 비트가 반전되는 특성이 있다.
  - 이 특성을 이용하면 보수를 구할 수 있고, 보수를 이용해 덧셈으로 뺄셈을 구현할 수 있다.
  - Bitwise Complement of Any Number N is -(N+1)
  ```
  Decimal            Binary          
    35              00100011
    
  35의 비트 보수 연산
  ~ 00100011
    11011100   =  Decimal 220
  ```
  
### 비트 시프트 연산자

### 비트 필드






























