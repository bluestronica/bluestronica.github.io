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








# 공용체










