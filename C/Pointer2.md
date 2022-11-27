[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 포인터 2

### 포인터와 배열의 차이
- sizeof 연산자
  - sizeof(배열)과 sizeof(포인터)는 다른 값을 반환
    - sizeof(배열): 배열의 총 크기를 반환
    - sizeof(포인터): 포인터의 크기를 반환 
- 문자열의 초기화
  - C는 C#이나 java처럼 문자열(string) 자료형이 없다.
  - char 배열을 이용해서 문자열을 표현
  - 문자열이 끝나는 지점을 알려주기 위해 널 문자(null character)를 항상 맨 마지막에 넣어줌
  - 널 문자: 값은 0으로 '\0'으로 표현
