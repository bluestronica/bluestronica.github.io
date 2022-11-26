[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 포인터 1

### 주소 연산자 &
- num이란 변수가 있으면 &num은 그 변수가 위치한 메모리 주소이다.
- 보통 주소를 보여줄 때는 16진수를 사용
  - 16진수는 4bit로 표현, 4bit*8byte = 32bit, **005AF7AC**
  - 그래서 printf()에서 서식 문자 %p는 주소를 16진수로 보여준다.
  - 참고: 실행 할 때마다 주소가 달라질 수 있음
  
### 포인터(pointer)
- **메모리 주소를 저장하는 변수**
- 포인터 변수를 선언하는 법
  - 자료형에 **`*`** 을 붙인다.
  - **`int*`, `char*`, `float*`**
  - **`int* address;`**
