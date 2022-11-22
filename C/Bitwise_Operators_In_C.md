[bluestronica.github.io/C]
(https://bluestronica.github.io/C)
https://turtlog.tistory.com/8
https://post.naver.com/viewer/postView.naver?volumeNo=23072744&memberNo=25379965&vType=VERTICAL

# Bitwise Operators In C


### 비트 연산자
- Bitwise **`AND`** Operator **`&`**
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
- Bitwise **`OR`** Operator **`|`**
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
- Bitwise **`XOR`** (exclusive OR) Operator **`^`**
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
- Bitwise **`Complement`** Operator **`~`**
  - 모든 비트가 반전되는 특성이 있다.
  ```
  35 = 00100011 (In Binary)

  Bitwise complement Operation of 35
  ~ 00100011 
    ________
    11011100  = 220 (In decimal)
  ```
  - 이 특성을 이용하면 보수를 구할 수 있고, 보수를 이용해 덧셈으로 뺄셈을 구현할 수 있다.
    1. The bitwise complement of 35 is 220 (in decimal). 
    2. The 2's complement of 220 is -36.
    3. Hence, the output is -36 instead of 220.
    4. Bitwise Complement of Any Number N is **-(N+1)**
  ```
   Decimal        Binary          2's complement 
     0           00000000          -(11111111+1) = -00000000 = -0(decimal)
     1           00000001          -(11111110+1) = -11111111 = -256(decimal)
     12          00001100          -(11110011+1) = -11110100 = -244(decimal)
     220         11011100          -(00100011+1) = -00100100 = -36(decimal)

  Note: Overflow is ignored while computing 2's complement.
  ```
  ```c
  int main(void)
  {
    char aValue = 0x86;     // 1000 0110  DEC 134가 아니라 -122 
                            // signed char 이기 때문에 숫자 범위는 -128~127
    char bValue = 0x1d;     // 0001 1101  DEC 29
    char cValue = ~bValue;  // 1110 0010  DEC -30 = -(29+1) = -(N+1) = ~29

    printf("aValue - bValue = %d\n", aValue - bValue);  // -151
    
    // C에서 보수를 이용해 덧셈으로 뺄셈을 구현 = (~N + 1)
    printf("aValue + (~bValue + 1) = %d\n", aValue + (~bValue + 1)); // -151


    printf("Output = %d\n", ~35);   // Output = -36
    printf("Output = %d\n", ~-12);  // Output = 11
    
    return 0;
  }
  ```

### 시프트 연산자
- Right Shift Operator **`>>`**
  - 모든 비트를 지정된 비트만큼 오른쪽으로 이동
  ```
  212 = 11010100 (In binary)
  212 >> 2 = 00110101 (In binary) [Right shift by two bits]
  212 >> 7 = 00000001 (In binary)
  212 >> 8 = 00000000 
  212 >> 0 = 11010100 (No Shift)
  ```
- Left Shift Operator **`<<`**
  - 모든 비트를 지정된 비트만큼 왼쪽으로 이동
  ```
  212 = 11010100 (In binary)
  212<<1 = 110101000 (In binary) [Left shift by one bit]
  212<<0 = 11010100 (Shift by 0)
  212<<4 = 110101000000 (In binary) =3392(In decimal)
  ```
- 시프트 연산의 결과로 비어있는 부분은 채워지고 **넘치는 부분은 손실**이 생긴다.
```c
int main()
{
  unsigned char data;
  
  data = 0x55;  // 0101 0101  (85)
  data <<= 1;   // 1010 1010  (170)
  data <<= 2;   // 1010 1000  (168)   overflow 발생 - 데이터 손실
  data <<= 3;   // 0100 0000  (64)    overflow 발생 - 데이터 손실
  
  
  data = 0xAA;  // 1010 1010 (170)
  data >>= 1;   // 0101 0101 (85)
  data >>= 2;   // 0001 0101 (21)   underflow 발생 - 데이터 손실
  data >>= 3;   // 0000 0010 (2)    underflow 발생 - 데이터 손실
  
  return 0;
}
```
- 시프트 연산자를 사용하는 방법은 2가지가 있다.
  - 특정 위치의 비트 값을 뽑아내거나 조작하는데 사용한다.
  ```c
  int main(void)
  {
    unsigned char data;

    // data에서 3번 비트만 볼 때
    data = 0x75;	// 0101 0101 (85)
    data <<= 4;		// 0101 0000 (80) 
    data >>= 7;		// 0000 0000 (0)
    printf("3번 비트 : %d\n", data);		// 0


    // data에서 6번 비트만 볼 때
    data = 0x55;// 0101 0101 (85);
    data <<= 1; // 1010 0000
    data >>= 7; // 0000 0001
    printf("6번 비트 : %d\n", data);   // 1
    
    return 0;
  }  
  ```
  - data의 특정 값을 곱셈 혹은 나누셈을 하고 싶은 경우 시프트 연산자를 이용할 수 있다.
    - `a<<b`는 `a * pow(2,b)`와 결과값이 완전히 같다.
      - a<<b = a의 특정 값이 2의 b승 만큼 이동한 값 = a * pow(2,b)
      - `pow(x,y)` 함수는 x를 y제곱한 값을 구하는 함수
    - `a>>b`는 `a * pow(2,-b)`와 결과값이 완전히 같다.
      - a>>b = a의 특정 값이 2의 -b승 만큼 이동한 값 = a * pow(2,-b)
    - 곱연산 대신 시시트연산 사용하기 - 2의 n승의 곱
    ```c  
    int IntegerPow(int bottom, int idx) 
    {
        while (idx-- != 0) 
        {
            bottom *= 2;  // data * 2^i = pow(2,i)
        }
        return bottom;
    }

    int main(void) 
    {
        unsigned char data = 0x09; // 0000 1001 (9)

        for (int i = 1; i <= 3; i++) 
        {
            if ((data << i) == IntegerPow(data, i))
            {
                //data<<1, data<<2, data<<3
                printf("결과는 %d로 같다\n", data << i); // 18,36,72 순으로 출력
            }
        }
        
        return 0;
    }
    ```
    - 곱연산 대신 시프트연산 사용하기 - 임의의 정수의 곱
    ```c
    int main(void){
        unsigned char data = 0x09; // 0000 1001 (9)
        int result1, result2;
        
        result1 = data * 10; // 10 = 2^3 + 2^1
        result2 = (data<<3) + (data<<1); // 2^3은 <<3, 2^1은 <<1
        
        if( result1 == result2 )
        {
            printf("결과는 %d로 같다\n", result1); // 90 출력
        }
        
        return 0;
    }    
    ```
    - 나눗셈연산 대신 시프트 연산 사용하기 - 부호가 없는 자료형
      - 부호가 있는 자료형의 경우 MSB의 비트에 따라 예외처리가 필요하게 된다. 따라서 나눗셈 연산자에 비해 시프트연산자가 얻는 이득은 거의 없으므로 그냥 나눗셈 연산자를 사용하는 것을 권장한다.
    ```c
    int main(void)
    {
        int result1, result2;
        unsigned char data = 0x99; // 1001 1001 (153)

        result1 = data / 4;
        result2 = data >> 2;
        if (result1 == result2)
        {
            printf("결과는 %d로 같다.\n", result1); // 38 출력
        }

        result1 = data / 16;
        result2 = data >> 4;
        if (result1 == result2)
        {
            printf("결과는 %d로 같다.\n", result1); // 9 출력
        }

        return 0;
    }    
    ```
    - 주의사항
      - 자료형의 크기를 넘어가게 될 경우 오버플로우 혹은 언더플로우가 발생할 수 있다.
      - 자료형에 부호 여부에 따라 MSB에 채워질 수 있는 비트가 달라지므로 주의해야한다.
      - 연산자 우선순위가 낮기 때문에 아래와 같은 상황이 발생할 수 있다.
        ```c
        result2 = data<<3 + data<<1       // error
        result2 = (data<<3) + (data<<1)   // ok
        ```

### 비트마스크 연산
- 비트마스크는 특정 위치의 비트 값을 뽑아내거나 조작하는데 사용되는 개념이다.
- 특정 비트 추출
  - 비트별 논리 연산(&)을 한다.
    - x & 0011 의미는 x의 값이 무엇이든 결과값의 세 번째 비트와 네 번째 비트는 항상 0이 될것이고, **첫 번째와 두 번째 비트 값만 알고 싶고**(추출) 나머지 비트는 0으로 처리해서 의미가 없는 값이 되는 것이다. 
- 특정 비트 조작
    - 해당 비트 값이 무없인지 모르지만 무조건 1 또는 0으로 만들거나, 비트를 반전시키는 용도로 사용
    - 특정 비트를 1로 변환
      - x | 0011 의미는 바꾸자 하는 비트를 1로 두고 비트별 논리합 연산(|)을 한다. x의 해당 비트가 0이든 1이든 결과 값으로 1이 나오게 하기 위해서는 1과 논리합 연산을 해야한다.
    - 특정 비트를 0으로 변환
      - x & 1100 의미는 x의 해당 비트가 0이든 1이든 결과값으로 0이 나오게 하기 위해서는 0과 논리곱 연산을 해야한다.
- 득정 비트를 반전
```c
#include<stdio.h>

int main() 
{
	int x = 10; // 이진수 표현: 1010
	int bitmask = (1 << 0) | (1 << 1); // 첫 번째 비트와 두 번째 비트가 1인 숫자: 0011
	// 

	printf("%d\n", x & bitmask); //특정 비트 추출, 결과: 0010 (출력값: 2)
	printf("%d\n", x | bitmask); //특정 비트를 1로 변환, 결과: 1011 (출력값: 11)
	printf("%d\n", x & ~bitmask); //특정 비트를 0으로 변환, 결과: 1000 (출력값: 8)
	printf("%d\n", x ^ bitmask); //특정 비트를 반전, 결과: 1001 (출력값: 9)

	return 0;
}
```


### 비트 필드






























