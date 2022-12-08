[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 가변 인자 함수(Variadic Function)
```c
<반환형> <함수명(<자료형이_정해진_매개변수_목록>, ...);
```
- 정해지지 않은 수의 매개변수(가변 인자)를 허용하는 함수!
  - 2개 넣어도 됨
  - 100개 넣어되 됨
- 반드시 최소 한 개의 정해진 자료형의 매개변수가 필요
- 가변 인자는 **`...`** 로 표
- 아주 많이 사용되진 않지만 가끔 유용한 경우가 있음
  - 메모리에 블록을 크게 잡아 두고 거기에 여러 가지 자료형을 저장하는 함수
  - printf() / scanf() 같은 것
```c
int add_ints(const size_t count, ...)
{
  va_list ap;
  int sum;
  size_t i;
  
  sum = 0;
  va_start(ap, count);
  {
    for (i = 0; i < count; ++i)
    {
      sum += va_arg(ap, int);
    }
  }
  va_end(ap);
  
  return sum;
}

int main(void)
{
  int result;
  
  result = add_ints(1, 16);
  printf("result: %d\n", result);
  
  result = add_ints(4, 1, 5, 6, 7);
  printf("result: %d\n", result);
  
  return 0;
}
```
- va_list
  - 가변 인자 목록
  - va_start(), va_arg(), va_end() 매크로 함수를 사용할 때 필요한 정보가 포함
  - 명시되지 않은 자료형 (구현마다 다름)
- va_start()
  - **`va_start(<가변 인자 목록>, <가변 인자 시작하기 직전 매개변수>);`**
  - 매크로 함수
  - 함수 매개변수로 들어온 가변 인자들에 접근하기 전에 반드시 호출해야 함
  - va_list에 필요한 초기화를 수행
    - 특히 가변 인자가 스택 메모리의 어디서부터 시작하는지 찾아냄
    - 그래서 두 번째 매개변수가 필요
- va_end()
  - **`va_end(<가변 인자 목록>);`**
  - 매크로 함수
  - 함수 매개변수로 들어온 가변 인자들에 접근이 끝난 뒤에 반드시 호출해야 함
  - 사용했던 가변 인자 목록을 정리함
    - 더 이상 가변 인자 목록을 사용할 수 없도록 가변 인자 목록의 값을 수정함
- va_arg()
  - **`va_arg(<가변 인자 목록>, <얻어올 가변 인자의 자료형>);`**
  - 매크로 함수
  - 가변 인자 목록으로부터 다음 가변 인자를 가져옴
  - 가져올 가변 인자의 자료형은 두 번째 매개변수로 알려줌
  - 예전 표준상의 문제로 가변 인자 목록의 기본 자료형 인자들은 다음과 같이 승격 됨
    - 모든 정수형은 int로
    - 모든 부동소수점은 double로
  - 따라서, 두 번째 매개변수에는 int나 double을 쓸 것
- 구조체도 가변 인자로 넣을 수 있음
```c
typedef struc
{
  int num;
  float f;  
} number_t;

void do_something(const size_t count, ...)
{
  va_list ap;
  number_t n;
  
  va_start(ap, count);
  {
    n = va_arg(ap, number_t);    // 구조체
    // 코드 생략 ..
  }
  va_end(ap);
}

number_t nums = { 10, 3.14f };
do_something(1, nums);
```

### va_arg()는 매크로 함수
- 함수처럼 보이지만 엄밀한 의미의 함수는 아님
  - 스택 프레임을 만들지도
  - 매겨변수를 전달하지도
  - 함수 주소로 점프하지도 않음
- 그대신 전처리가 매크로 함수의 구현 코드로 대체시켜 줌
```c
val = va_arg(ap, int);
```
```c
val = *(int*)ap.data; 
((int*)ap.data)++;    
```

### 어떻게 다른 수의 매개변수를 스택에 넣을까?
- 어떤 함수를 호출할 때마다 매개변수를 순서대로 스택에 넣어준다.
- 하지만 가변 인자 함수는 매번 매개변수 개수가 바뀌는데 어떻게?
- 가변 인자 함수를 호출하는 호출자는 매개변수 몇개를 넣어야하는지 알고있다.
  - **`add_ints(1, 16);`** : 2개
  - **`add_ints(4, 1, 2, 3, 4);`** : 5개
  - 따라서 그냥 다 넣어줌

### 첫 번째 매개변수가 저장된 스택 메모리 위치는?
- add_ints() 함수의 스택 프레임 바로 전
- 따라서 호출받은 함수가 어디서 첫 번째 변수를 읽어와야 하는지 앎
- 호출받은 함수가 몇 개의 인자를 받는지 모른다.
- 그래서 첫 번째 변수가 몇 개의 인자를 받는지 나타내는 정보를 담는 변수이고
- 그 함수의 스택 프레임 바로 전에 저장되기 때문에 그 위치에 가서 값을 읽어 몇 개의 변수를 받는지 알아 낼 수가 있다.
- 메모리 :  **`| add_ints() 스택 프레임 | 돌아올 주소 | 4 | 1 | 2 | 3 | 4 |`**

### 가변 인자 함수는 이렇게 인자를 읽어온다!
- **`va_start(ap, count)`** 에서 가변 인자 시작 직전 매개변수(int 형)에 기초해서 
- 가변 인자 목록의 시작 메모리 주소를 계산한다.
  - **`va_start(ap, count);  =>  ap.data = (char*)&count + sizeof(int);`**
- 메모리 :  **`| add_ints() 스택 프레임 | 돌아올 주소 | 4(count) | 1 | 2 | 3 | 4 |`**
- count의 시작주소를 char*로 캐스팅해야 순수하게 바이트로 4바이트씩 이동할 수 있다.
- **`count의 시작주소 ->|1byte|1byte|1byte|1byte|1(4byte)|2(4byte)|3(4byte)|4(4byte)|`**
- count의 시작주소에서 이 count의 길이만큼 바이트 수(int:4byte)를 옮기면 된다.
- count의 길이는 sizeof(int)이다. 왜냐하면 int형이 들어온 걸 알고 있으니까.
- 실체 코드에서는 sizeof(count)를 쓸 일이 많다. 변수를 가져다 sizeof()에 넣어도 똑같다.


























