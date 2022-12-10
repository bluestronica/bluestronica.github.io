[bluestronica.github.io/C](https://bluestronica.github.io/C)


# 동적 메모리

### 프로그램이 동적 메모리를 가져다 사용할 때는 총 세 가지 단계를 거친다.
- 메모리 할당
- 메모리 사용
- 메모리 해제

### 메모리 할당 함수: malloc()
- ` void* malloc(size_t size); `
- 메모리 할당(memory allocation)의 약자
- size 바이트 만큼 메모리를 반환해줌
- 초기화를 안 해주기때문에 반환된 메모리에 들어있는 값은 쓰레기 값
- 메모리가 더 이상 없다거나 해서 실패하면 NULL 반환

### malloc()의 짝꿍 함수 free()
- 메모리를 빌렸으면 메모리를 반납해야 함
- **고로 malloc() 코드를 작성하면 곧바로 free() 코드도 추가하는 습관을 들이는게 좋음**
- malloc()한 뒤 free() 까먹으면 메모리 누수 발생 
- 그래서 malloc()과 free()는 한 몸이다!!!

### 할당받은 메모리를 해제 - free()
- ` void free(void* ptr); `
- 할당받은 메모리를 해제하는 함수
- 즉, 메모리 할당 함수들을 통해서 얻은 메모리만 해제 가능
- 그 외의 주소를 매개변수로 전달할 경우 결과가 정의되지 않음

### 할당받아 온 주소를 그대로 연산에 사용하면?
```c
int* nums;

nums = malloc(LENGTH * sizeof(int));

for (i = 0; i < LENGTH; ++i)
{
  *nums++ = 10 * (i + 1);  // 할당받은 포인터로 연산 금지
  // 반환된 주소를 가지고 있는 변수를 그대로 포인터 연산에 사용하면 
  // 메모리 해제할 때 문제가 발생할 수도 있음
  // 최초에 받아온 주소가 아니라 다른 위치를 가리키게 된다.
  // 그 주소로 메모리 해제 요청하면 엉뚱한 주소를 해제하게 된다.
}

free(nums);  // 엉뚱한 주소를 해제
```

### 할당받은 포인터로 연산 금지!
```c
void* nums;
int* p;
size_ i;

nums = malloc(LENGTH * sizeof(int));
p = nums;     // 메모리 할당 함수에서 받아온 포인터와 
              // 포인터 연산에 사용하는 포인터를 분리하자

for (i = 0; i < LENGTH; ++i)
{
  *p++ = 10 * (i + 1);
}

free(nums);
```

### 메모리 해제 후 널 포인터를 대입
- free()한 뒤에 변수에 NULL을 대입해서 초기화
  - 안 그러면 해제된 놈인지 나중에 모르니
  - 널 포인터를 free()의 매개변수로 전달해도 안전
```c
void* nums;
int* p;
size_ i;

nums = malloc(LENGTH * sizeof(int));
p = nums; 

// 코드 생략

free(nums);
nums = NULL;    // 해제 후 NULL 대입해서 초기화
```

### free()는 몇 바이트를 해제할 지 어떻게 알까?
- 구현마다 다르지만 보통 malloc(32)하면 그것보다 조금 큰 메모리를 할당한 뒤, 제일 앞부분에 어떤 데이터들 채워 놓음
  - `| ... | 어떤 데이터 | 32바이트짜리 메모리 | ... |`
- 그리고 그만큼 오프셋을 더한 값을 주소로 돌려줌
  - 만약 '어떤 데이터'의 크기가 4바이트면 0x100에 4만큼 더한 0x104를 돌려줌
- 나중에 그 주소를 해제 요청하면 free()가 다시 오스펫만큼 빼서 그 앞 주소를 본 뒤, 실제 몇 바이트가 할당됐는지 확인('어떤 데이터' 내용을 확인) 후 해제 

### 메모리 할당 함수: calloc()
- `void* calloc(size_t num, size_t size);`
- 메모리르 할당할 때 자료형의 크기(size)와 수(num)를 따로 지정
- 모든 바이트를 0으로 초기화 해 줌
- 잘 안 씀
- **calloc() 대신 malloc() + memset()를 조합해서 쓴다.**
 - memset()을 쓰면 0 외의 값으로도 초기화 가능

### 메모리 할당 초기화 함수: memset()
- `void* memset(void* dest, int ch, size_t count);`
  - dest는 메모리 시작 위치, count는 메모리 총 바이트, ch는 초기화할 값 
- 무조건 char로 초기화(1바이트 씩)됨
```c
void* nums;

nums = malloc(LENGTH * sizeof(int));
memset(nums, 1000, LENGTH * sizeof(int));  // 1000의 값은 16진수로 0x3E8

free(nums);
nums = NULL:

// ch의 값(1000)이 1바이트를 넘어 선다. 
// 그래서 03은 날아가고 E8 값으로 초기화 된다.
```
- 그 외의 자료형으로 초기화하려면 직접 for 문을 써야함
```c
void* nums;
int* p;
size_t i;

nums = malloc(LENGTH * sizeof(int));
p = nums;

for (i = 0; i < LENGTH; ++i)
{
  *p++ = 1000;   // (int)4바이트 씩 초기화 값 03E800 들어간다.
}

free(nums);
nums = NULL;
```
- 다름 과 같은 경우 결과가 정의되지 않는다.
 - count가 dest의 영역을 넘어설 경우(소유하지 않은 메모리에 쓰기)
 - dest가 널 포인터일 경우( 널 포인터 역참조)






























