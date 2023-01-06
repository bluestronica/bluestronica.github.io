# Singleton Pattern

### 동적할당을 이용해서 싱글톤 만드는 방식
- 개체는 힙 메모리에 할당
- 개체를 가리키는 주소는 데이터 섹션에 저장
- 개체 주소의 유무를 통해 개체의 개수를 통제한다.
- 정적 맴버함수로 바로 접근해서 개체를 생성
  - 개체(힙)와 정적 멤버함수(데이터 섹션)는 하나로 묶여있는 데이터 덩어리가 아니라 서로 저장 위치가 다른 별개의 데이터 덩어리다.
  - 그래서 개체를 생성하지 않고 정적 멤버(함수, 변수) 호출이 가능하다.
  - 문법으로 표현하면 범위지정 연산자를 통해서 클래스안에서 접근을 한 것이다.
    - `CClass::GetInstance();`
  - 이렇게 정적 멤버는 반드시 클래스 선언 밖에서 반드시 초기화 해야한다.
  - 데이터 섹션에 할당 될 멤버의 초기값을 반드시 명시 해줘야한다.
  - 초기화는 cpp 파일에서 해준다.
  - `CCore* pCore = CCore::GetInstance();`
    - 개체를 생성하고 개체 주소를 받아온다. 
    - 다음에 또 받아와도 이미 생성된 개체의 주소를 받아오게 된다.
    - 그래서 하나의 개체만 생성된 상태가 되는 것이다.
```c++
class CCore
{
private:
	// 정적 맴버변수로 만든 이유는
	// 개체를 생성하지 않고 정적 멤버함수에 접근 하는데,
	// 정적 멤버함수는 정적 멤버만이 접근 할 수 있기때문에 
	// 정적 맴버변수를 사용한다.
	// 즉, 개체 생성 없이 데이터섹션 데이타(정적 멤버함수)로 접근해서
	// 데이터섹션 데이타(정적 멤버변수)에 저장된 값을 이용한다.
	// 정적멤버함수 GetInstance, Release 둘 다 접근 가능한 
	// 정적 멤버변수를 만들기 위해 정적 멤버함수 밖으로 끄집어내서 만든것이다.
	static CCore* g_pInst; 

public:
	// 정적 멤버함수로 만든 이유는
	// 개체를 바로 생성하지 않고 정적 멤버함수에 바로 접근해서 
	// 개체를 생성하기 위함이다.
	static CCore* GetInstance()
	{
		// 최초 호출 된 경우
		if (nullptr == g_pInst)
		{
			g_pInst = new CCore; // 개체 주소를 데이터 섹션에 저장
		}                            // 개체는 동적할당(힙) 되어 있다.

		return g_pInst;
	}

	// 동적할당을 해줬으니 메모리 직접 해제해줘야 한다.
	// 물론 개체 없이 호출 가능해야한다. 
	// 그래서 정적 멤버함수로 만든다.
	static void Release()
	{
		if (nullptr != g_pInst)
		{
			delete g_pInst;
			g_pInst = nullptr;  // 삭제하고 다시 생성했을 때,
					    // nullptr이면 다시 생성 해준다.
		}
	}

private:
	// 생성자를 private으로 막았기 때문에 외부에선 더 이상 CCore라는
	// 클래스를 생성할 수 없다. 생성자가 호출이 안되기 때문이다.
	CCore();
	~CCore();
};
```

### 싱글톤으로 만들 유일한 개체를 데이터 섹션에 올리는 방식
- static은 데이터 섹션에 저장된다. 
- `static CCore mgr;` 
- 그래서 개체는 데이터 섹션에 만들어지게 된다.
- 정적 변수는 한번만 초기화되고 그 다음부터는 무시된다.
- 그래서 앞으로 호출될 때마다 즉시, 데이터 섹션에 있는 개체의 주소를 넘겨주게된다.
- 정적 변수가 해당 정적함수에서만 접근 가능하지만
- 주소를 통해서 접근하는 것은 불가능한 것이 아니다.
- 그래서 주소 받은 쪽에서 그 주소로 접근하는 것이 가능하다.(포인터의 장점)
- 해제를 신경쓰지 않아도 된다. 프로그램이 종료될까지 데이터가 남는다.
```c++
class CCore
{
public:
	static CCore* GetInst()
	{
		static CCore mgr;  // 개체를 데이터 섹션에 만든다.
		return &mgr;       // 데이터 섹션에 저장된 그 개체 주소를 전달
	}
	
private:
	CCore();
	~CCore();
};
```


# 싱글톤 클래스를 매크로 함수로 만들기
- 매크로 함수 
  - 함수가 호출되는 것이 아니라 **코드를 치환하는 형태**이다. 
  - 자주 반복되는 코드 유형 패턴을 미리 만들어 놓고 필요 할 때마다 쓰면 좋다.
- Engine/Header/Define.h
  - 메크로나 어떤 정의의 것들을 모아 놓은 헤더파일
```c++
// Singleton 메크로
//#define SINGLE(type) static type* GetInst() { static type mgr; return &mgr; }
#define SINGLE(type) public:\
			 static type* GetInst()\
			 {\
				 static type mgr;\
				 return &mgr;\
			 }
```
```c++
class CCore
{
	SINGLE(CCore);

private:
	CCore();
	~CCore();
};
```


# 미리 컴파일된 헤더 - pch.h
- 미리 컴파일된 헤더라는게 생겨나면
- 모든 cpp 파일들은 무조건 미리 컴파일된 헤더를 참조해야한다.
- 속성에 들어가서 미리 컴파일된 헤더 체크 확인!
```c++
// 미리 컴파일된 헤더
#include "Windows.h>

#include "define.h"
```



# Core Class 초기화
- CCore.h
```c++
class CCore
{
	SINGLE(CCore);

private:
	HWND	m_hWnd;		 // 메인 윈도우 핸들
	POINT	m_ptResolution;  // 메인 윈도우 해상도

public:
	int init(HWND _hWnd, POINT _ptResolution); // 실패 체크하기 위해 정수값 하나 반환
	void progress();

private:
	CCore();
	~CCore();
};
```
- CCore.cpp
```c++
int CCore::init(HWND _hWnd, POINT _ptResolution)
{
	m_hWnd = _hWnd;
	m_ptResolution = _ptResolution;

	// 해상도에 맞게 윈도우 크기 조정

	return S_OK;
}
```

- main.cpp
```c++
// Core 초기화 : 윈도우에서 사용하는 기법인 FAILED을 가져와서 구조를 만든다.
if (FAILED(CCore::GetInst()->init(g_hWnd, POINT{1280, 768})))
{
	MessageBox(nullptr, L"Core 개체 초기화 실패", L"ERROR", MB_OK);

	return FALSE;
}
```

# 윈도우 스타일 기법 - FAILED
- 성공 했을 때
  - `return S_OK;`
  - S_OK는 매크로로 `((HRESUKT)0L)` 정의되어 있다. **0**이다.
- 실패 했을 때
  - `return F_FAIL;`
  - F_FAIL은 매크로로 `_HRESULT_TYPEDEF_(0x80004005L)`
    - 16진수 첫 자리 비트가 8이므로 음수이다. 
    - 최상의 비트가 1이니깐 8은 음수이다.
- 윈도우에서 자주 사용하는 FAILED 매크로
  - `#define FAILED(hr) (((HRESULT)(hr)) < 0)`
  - hr이 0보다 작은 음수면 true를 반환한다.
  - 그래서 대표적인 음수 값인 E_FAIL이 들어가 있으면
  - `FAILED(E_FAIL)`
  - 참으로 뜬다.
  ```c++
  if (FAILED(E_FAIL))  // 그래서 FAILED 실패했다면 true 이므로
  {
      실행
  }

  if (FAILED(S_OK))  // FAILED에 0반환 되므로 false 뜨면서 
  {
      살행 안함
  }
  ```


















