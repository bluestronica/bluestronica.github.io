[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 구조체
### C에서 구조체는?
- 데이터의 집합
- 여러 자료형을 가진 변수들을 하나의 패키지로 만들어 놓은 것
- 참고로 C에서는 모든 데이터가 동일하게 값형이다. 다 복사해서 들어가는 것들이다. 참조형처럼 쓰고 싶다면 주소를 전달하면된다.

### 구조체 선언
```c
struct data       // date란 구조체(새로운 형)을 만든다.
{
  int year;      // 구조체 선언과 동시에 초기화 안 됨
  int month;
  int day;
};

struct date date;   // 변수명은 date

date.year = 2043;   // 멤버 변수에 접근은 .연산자를 사용한다.
date.month = 10;
date.day = 1;
```

### 새로운 별명을 지어주는 typedef
```c
typedef unsigned int  size_t;
```
- 이미 있는 자료형인 unsigned int에 새로운 별명을 지어줌
- 그 새로운 이름은 size_t
- 참고로 C에서 _t로 끝나는 자료형은 보통 이렇게 typedef한 것이다.

### 구조체에 typedef 적용
- 사용법 1
```c
struct data       
{
  int year;
  int month;
  int day;
};

typedef struct date date_t; 

date_t date;

date.year = 2043; 
date.month = 10;
date.day = 1;
```
- 사용법 2
```c
typedef struct data       
{
  int year;
  int month;
  int day;
} date_t;

date_t date;

date.year = 2043; 
date.month = 10;
date.day = 1;
```
- 사용법 3 
```c
typedef struct     
{
  int year;
  int month;
  int day;
} date_t;

date_t date;

date.year = 2043; 
date.month = 10;
date.day = 1;
```

### 커스텀 자료형에 typedef를 쓰자!
- 가능하면 구조체, 열거형, 공용체에 typedef를 써서 정말 제대로 된 자료형처럼 보이게 하자!

### 구조체 포인터에서 맴버의 값에 접근하기
- 구조체를 인자로 넘겨 원본을 바꾸고 싶다면 구조체 포인터를 전달하면된다.
  - 원본을 바꾼다 -> 포인터
```c
void increase_year(date_t* date)
{
  // 세 코드 모두 같은 의미!
  
  (*date).year = (*date).year + 1;
  
  date->year = date->year + 1;
  
  date->year++;
}
```
- 점(`.`)은 어떤 구조체 변수의 멤버 변수에 접근하는 것이다.
- 근데 그 구조체가 구조체로 직접 값으로 들어온 게 아니라 주소로 들어오면
- **그 주소가 가리키는 구조체에 접근을 해서 그 다음 멤버 변수에 접근한다는 것이다.**
- 이 2개를 합친게 화살표(`->`)
- 즉, date가 포인터라면
  - 화살표(`->`)를 써야되는 거고
- date가 값이라면
  - 그냥 점(`.`)만 쓰면 된다.

### 구조체 매개변수 베스트 프랙티스
- 값으로 전달 vs 주소로 전달
  - 기본 자료형 전달할 때는 간단했음
    - 기본 데이터 크기가 작으니 원본 바꿀 때만 주소로 전달하면 된다.
  - 구조체의 경우 데이터 크기가 클 수도 있음
    - 이럴 때 주소로 전달! 
    - 포인터에 const 포인터를 붙이면 원본도 못 바꾸니 안전 
- 구조체 매개변수 vs 여러 개의 개별 변수?
  - 딱히 정확한 규칙이 있지 않지만 보통 변수 많이 전달하는 대신 구조체 하나 전달하라고 한다.
  - 한 4개까지는 낱개 변수로 그 이후에는 구조체로 넘기라는 규칙을 쓰는 회사도 있음 

### 함수 반환값으로서의 구조체
```c
date_t get_dday(void)
{
  date_t date;

  date.year = 2043; 
  date.month = 10;
  date.day = 1;
  
  return date;
}

date_t date;

date = get_dday();
```
- int형 하나하나 반환하는 것과 마찬가지로 복사에 의해 반환!
- C언어의 함수는 반환값이 하나
- 이렇게 구조체를 반환하면 실질적으로 여러 개의 값을 반환하는 격이다.

### 구조체로 배열도 만들 수 있다.
- 구조체를 정말 기본 자료형이랑 다르지 않다고 생각하는게 편함
- 구조체마다 자료 크기가 딱 정해져 있으니 컴파일러가 다른 변수와 똑같이 처리해줄 수 있음
```c
date_t family_birthdates[4];

family_birthdates[0].year = 1950;
family_birthdates[1].month = 1;
family_birthdates[2].day = 2;

...
```

### 얕은 복사
- 포인터 변수에 대입한 값은 주소!
- 실제 데이터가 아니라 주소를 복사하는 걸 얕은 복사라고 한다.

### 깊은 복사
- 구조체 변수마다 독자적인 메모리 공간을 만들어주고 거기에 문자열을 복사
- 동적 메모리에서 배운다.

### 파일을 읽고 쓸 때도 비슷한 문제가 발생한다.
```c
typedef strtuct nmae
{
  char* lastname;
  char* firstname;
} name_t;

name_t names[NUM_NAMES];
FILE* stream;

names[0].firstname = "Teemo";
names[0].lastname = "Kim";
names[1].firstname = "LULu";
names[1].lastname = "Lee";

stream = fopen("names.txt", "wb");

fWrite(names, sizeof(names[0], NUM_NAMES, stream);
fflush(stream);

fclose(stream);
```
- 구조체 배열을 만들어 데이터를 넣고 그것을 통째로 파일에 저장을 하게되면 문자열로 저장한게 아니라 그 문자열의 시작 주소(names)만 저장한 것이다. 프로그램 재실행해서 주소를 읽어와 보면 그 문자열이 똑같은 위치에 들어가 있지 않다. 문제가 발생한다.

### 그래서 포인터 대신 배열로 구조체 작성
```c
enum { NAME_LEN = 32 };
typedef struct
{
  char firstname[NAME_LEN];
  char lastname[NAME_LEN];
} name_t;
``
- 가능한 한 덩어리 메모리에 모든 데이터가 들어가고 대입 가능한 구조체를 만드는게 좋음
  - 매개변수로 전달을 해도 배열로 작성한 구조체는 요소 내용 전부 다 스택에 복사해서 넣어준다.
  - 매개변수로 넣을 수 있는 것 자체가 구조체를 대입할 수 있다는 의미다.
  - 대입을 하는 순간 이 배열 속에 있는 모든 요소를 하나씩 다 복사해 간다.
- 즉, 포인터만 없으면 된다.

### 바이트 정렬 요구사항
```c
struct user_info
{
  unsigned int id;          // 4바이트
  name_t name;              // 64바이트
  unsigned short height;    // 2바이트
  float weight;             // 4바이트
  unsigned short age;       // 2바이트
};                          // 76바이트
                            // 실제 구조체 크기는 80바이트!!
                            // 2바이트를 4바이트 경계를 맞춤 
                            // 그래서 80바이트!!
```
- x86 시스템은 4바이트(워드 크기) 경계에서 읽어오는게 효율적
  - 이걸 4바이트 경계에 정렬된다(aligned)라고 말한다.
- 따라서 컴파일러가 알아서 각 멤버의 시작 위치를 그 경계에 맞춤
- 그러기 위해 안 쓰는 바이트를 덧붙임(padding)
- 32비트 clang 윈도우는 4바이트 정렬을 하려고 함
- 따라서 어떤 아키텍처에서 저장한 파일을 다른 아키텍처에서 읽으면 잘못 읽힐 수도 있음
- 구조체를 파일이나 다른 데 저장해야 해서 바이트 크기가 정확히 맞아야 한다면
  - 보통 assert()를 사용해서 크기를 확인한다.
  - `assert(sizeof(user_info_t) == 76);`
  - 특히 데이터의 전체 크기가 4바이트로 안 나눠 떨어질 때
    - 구조체에 패딩을 명시적으로 넣기도 함
    - `char unused[2];`

### 구조체와 비트 플래그
- 내가 필요한 비트 수만큼 쪼개쪼개 쪼개서 정확히 데이터를 패킹하겠다는 의미이다. 그래서 이런 것을 데이터 패킹이라고 한다.
- 4비트를 쓰면 총 16가지의 값, 0부터 15까지의 값을 표현할 수 있다.


# 공용체
### 똑같은 메모리 위치를 다른 변수로 접근하는 방법
```c
typedef union
{
  unsigned char val;
  struct
  {
    unsigned char b0  :  1;
    unsigned char b1  :  1;
    unsigned char b2  :  1;
    unsigned char b3  :  1;
    unsigned char b4  :  1;
    unsigned char b5  :  1;
    unsigned char b6  :  1;
    unsigned char b7  :  1;
  } bits;
} bitflags_t;

bitflags_t flags = { 0, };
```
- val로도 bits로도 접근이 가능함!
- flags의 주소가 100이라 가정
- flags.val의 시작주소 100
- bits.bo의 시작주소 100
- 동일한 메모리를 두 개의 서로 다른 자료형으로 접근
- 그러나 그 메모리 안에 있는 값은 동일하다!

### 공용체 안에 있는 여러 변수들이 같은 메모리를 공유
- 앞의 예보다 덜 유용
- 사용하기 어렵고 실수하기도 쉽다.
```c
value_t calculate(value_t lhs, value_t rhs, op_t op)
{
  value_t result;
  
  if (op == OP_INTADD)
  {
    result.ivalue = lhs.ivalue + rhs.ivalue;
  }
  else if ( op == OP_DOUBLEADD )
  {
    result.dvalue = lhs.dvalue + rhs.dvalue;
  }
  else
  {
    assert(0);
  }
  
  return result;
}

typedef union
{
  int ivalue;
  double dvalue;
} value_t;

value_t op1;
value_t op2;
value_t result;

op1.ivalue = 55;
op2.ivalue = 27;
result = calculate(op1, op2, OP_INTADD);

op1.dvalue = 10.3456;
op2.dvalue = 53.2343;
result = calculate(op1, op2, OP_DOUBLEADD);
```




