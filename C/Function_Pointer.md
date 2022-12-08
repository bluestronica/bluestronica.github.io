[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 함수 포인터
### 함수도 어디다 저장해 둔 뒤 매개변수로 잔달할 수 없을까?
```c
result = calculate(op1, op2, operator);   // operator = add()
result = calculate(op1, op2, operator);   // operator = sub()
result = calculate(op1, op2, operator);   // operator = mul()
result = calculate(op1, op2, operator);   // operator = div()
```
- 주소를 저장하는 데이터 타입을 포인터라 한다. 
- 함수 포인터라하면 어떤 데이터가 저장된 주소가 아니라 
- add() 함수의 시작코드가 있는 그 주소를 말한다.

### 함수 포인터 선언
**`<반환형> (*<변수명>)(<매개변수 목록>);`**
- 함수의 시작 주소를 저장하는 변수
- 함수의 매개변수 목록과 반환형을 반드시 표기해야 함

### 함수 포인터 변수의 선언과 사용
```c
double add(double x, double y)
{
  return x + y;
}

double (*func)(double, double) = add;
result = func(op1, op2);
```

### 함수 포인터 매개변수의 선언과 사용
```c
// 선언
double calculate(double, double, double (*)(double, double));

// 정의
double calculate(double x, double y, double (*func)(double, double))
{
  return func(x, y);
}

// 사용
result = calculate(op1, op2, add);
```


### 함수 포인터 읽는 방법
```c
double (*func)(double, double)
```
- 변수 func는 두 개의 double형 매개변수를 받고 double형을 반환하는 함수의 포인터
```c
void (*func)(int)
```
- 변수 func는 한 개의 int 형 매개변수를 받고 반환값이 없는 함수의 포인터

### 포인터 함수를 담는 배열
```c
int (*ops[])(int, int) = { add, sub, mul, div };
```
- ops는 배열이다.
  - 배열의 각 요소는 함수 포인터이며
    - 두 개의 int 형 매개변수를 받고
    - int 형을 반환한다.
- 즉, ops 배열은 포인터를 담고 그 포인터는 int 2개를 매개변수로 받는 함수를 가리키고 int 형을 반환한다.

### void*
- 범용적 포인터
- 어떤 포인터라도 여기에 대입 가능
  - 그 반대도 그냥 대입 가능
  - 따라서 어떤 변수의 주소라도 곧바로 대입 가능
  - 매개변수 형으로 void*를 사용하면 어떤 포인터도 받을 수 있는 함수 탄생
- 단, 다음과 같은 경우 다른 포인터로 캐스팅 또는 대입해서 써야 함
  - 역 참조(몇 바이트 읽을지 모르기 때문)
  - 포인터 산술 연산( 몇 바이트 이동할지 모르기 때문)
```c
int add(void* op1, void* op2)
{
  int result;
  result = *op1 + *op2; // 컴파일 오류: 역참조 (몇 바이트를 읽을지 모르기 때문에)
  result = *(int*)op1 + *(int*)op2;  // ok : 캐스팅하고 역참조하니 출력 가능
}

float pi = 3.14f;
void* p;
float* q;

p = &pi;  // void포인터에 float포인터 대입 가능
q = p;    // float포인터에 void포인터 대입 가능

printf("%f\n", *p); // 컴파일 오류: 역참조 (몇 바이트를 읽을지 모르기 때문에)
printf("%f\n", *(float*)p); // ok : 캐스팅하고 역참조하니 출력 가능
```



