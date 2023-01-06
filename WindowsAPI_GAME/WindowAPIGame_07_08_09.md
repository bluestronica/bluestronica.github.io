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
	int init(HWND _hWnd, POINT _ptResolution); // 실패 체크하기 위해 
						   // 정수값 하나 반환
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


# 해상도에 맞게 윈도우 크기 조정
- 해상도는 순수하게 물체가 그려지는 작업영역을 말한다.

### POINT _ptResolution
- 입력받은 해상도 사이즈 값이 들어 있다.
```c++
// 원하는 해상도 사이즈
RECT rt = {0, 0, m_ptResolution.x, m_ptResolution.y};
```

### AdjustWindowRect
- 해상도 사이즈로 타이틀바, 메뉴바, 테두리 등이 포함된 클라이언트 영역(윈도우 전체 크기)이 되도록 사이즈 조정한다.
```c++
AdjustWindowRect(
    _Inout_ LPRECT lpRect,
    _In_ DWORD dwStyle,
    _In_ BOOL bMenu);
    
// LPRECT : RECT 구조체를 가릴킬 수 있는 포인터 타입을 의미하는 것이다., *RECT;
// dwStyle : 비트 연산으로 Window Styles을 나타낸다. 
// 함수 결과는 값을 반환하는 것이 아니라 지역변수 rt에 값이 채워진다.
// _Inout_ : 입력과 결과를 되돌려 받는 두 가지 의미가 있다. 
// 리턴값이 너무 클 때 이런 방식으로 한다.
// 인자로 주소를 받아서 다시 접근해서 결과를 담아준다.    
```
```c++
AdjustWindowRect(&rt, WS_OVERLAPPEDWINDOW, true);
```

### SetWindowPos
- 윈도우 전체 크기를 세팅한다.
```c++
SetWindowPos(
    _In_ HWND hWnd,
    _In_opt_ HWND hWndInsertAfter,
    _In_ int X,
    _In_ int Y,
    _In_ int cx,	// 가로 : rt.right - rt.left
    _In_ int cy,	// 세로 : rt.bottom - rt.top
    _In_ UINT uFlags);
```
```c++	
SetWindowPos(m_hWnd, nullptr, 100, 100, 
  		rt.right - rt.left, rt.bottom - rt.top, 0);  
```

# 프로그램 실행!
- 메세지가 발생하지 않는 대부분의 시간을 갖는 그곳에서 프로그램이 실행되도록 한다.
- main.cpp
```c++
while (true)
    {                 
        // 반환 된 메세지가 무(없으면) fasle, 유(있으면) true
        if (PeekMessage(&msg, nullptr, 0, 0, PM_REMOVE))
        {            
            if (WM_QUIT == msg.message)
            {
                break;
            }

            if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
            {
                TranslateMessage(&msg);
                DispatchMessage(&msg);
            }
        }

        // 메세지가 발생하지 않는 대부분의 시간 
        else
        {            
            // Game 코드 수행
            // 디자인 패턴(설계 유형)
            // 싱글톤 패턴
            CCore::GetInst()->progress();
        }
    }
```
- CCore.cpp
```c++
void CCore::progress()
{
	// 메세지가 없을 때는 계속 작업한다.
	// 프로그램 실행!!!!
	static int callCount = 0;
	++callCount;

	static int iPrevCount = GetTickCount();  // 계속 데이타 지속

	int iCurCount = GetTickCount();  // 현재
	if (iCurCount - iPrevCount > 1000)   // 1초 차이가 났을 때
	{
		iPrevCount = iCurCount;
		callCount = 0;
	}
	
	update();  // 상태 변경 처리

	render();  // 처리된 값으로 그리기

	// 이제부터는 메세지 신경 쓸 이유 없다. 다시 c++로 넘어 왔다.
}

void CCore::update()
{
	// 물체들의 좌표나 변경점들을 다 해결하고 
	// 픽스된 상태에서 render()에서 다시 그려낸다.

	// 물체를 특정 키가 눌렀을 때 포지션 변경
	// 키 입력도 메세지 기반이 아니다. 키 눌린 순간 바로 체크
	// 비동기 키 입출력 함수
	if (GetAsyncKeyState(VK_LEFT) & 0x8000)
	{
		g_obj.m_ptPos.x -= 1;
	}
	
	if (GetAsyncKeyState(VK_RIGHT) & 0x8000)
	{
		g_obj.m_ptPos.x += 1;
	}
}

void CCore::render()
{
	// 그리기 
	//Rectangle(m_hDC, 10, 10, 110, 110);  
	// 실행 시켜보면 사각형 하나 그려져 있는데.. 
	// 실시간으로 계속 그려지고 있는 모습이다.

	Rectangle(m_hDC,
		g_obj.m_ptPos.x - g_obj.m_ptScale.x / 2,
		g_obj.m_ptPos.y - g_obj.m_ptScale.y / 2,
		g_obj.m_ptPos.x + g_obj.m_ptScale.x / 2,
		g_obj.m_ptPos.y + g_obj.m_ptScale.y / 2);
}
```

# 코드
### CCore.h
```c++
class CCore
{
	SINGLE(CCore);

private:
	HWND	m_hWnd;		// 메인 윈도우 핸들
	POINT	m_ptResolution; // 메인 윈도우 해상도
	HDC		m_hDC;	// 메인 윈도우에 Draw 할 DC

public:
	int init(HWND _hWnd, POINT _ptResolution);
	void progress();

private:
	void update();
	void render();

private:
	CCore();
	~CCore();
};
```

### CCore.cpp
```c++
#include "pch.h"
#include "CCore.h"

#include "CObject.h"

CObject g_obj;

CCore::CCore()
	: m_hWnd(0)
	, m_ptResolution{}
	, m_hDC(0)
{
}

CCore::~CCore()
{
	ReleaseDC(m_hWnd, m_hDC);
}

int CCore::init(HWND _hWnd, POINT _ptResolution)
{
	m_hWnd = _hWnd;
	m_ptResolution = _ptResolution;  
	
	RECT rt = {0, 0, m_ptResolution.x, m_ptResolution.y};  
	AdjustWindowRect(&rt, WS_OVERLAPPEDWINDOW, true);  
	SetWindowPos(m_hWnd, nullptr, 100, 100, 
			rt.right - rt.left, rt.bottom - rt.top, 0);  

	// 이제부터 메세지 기반 쪽 기능을 쓰지 않는다.
	// 메세지 처리가 발생하지 않는 시간 동안에 모두 처리를 다할 것이다.
	// 이제 앞으로 여기서 그리기 작업을 한다.
	// BeginPaint 사용하지 않고 GetDC 사용

	m_hDC = GetDC(m_hWnd);

	// 이니셜라이져로 초기값
	g_obj.m_ptPos = POINT{ m_ptResolution.x / 2, 
					m_ptResolution.y / 2 };
	g_obj.m_ptScale = POINT{ 100, 100 };

	return S_OK;
}

void CCore::progress()
{
	static int callCount = 0;
	++callCount;

	static int iPrevCount = GetTickCount();  // 계속 데이타 지속

	int iCurCount = GetTickCount();  // 현재
	if (iCurCount - iPrevCount > 1000)   // 1초 차이가 났을 때
	{
		iPrevCount = iCurCount;
		callCount = 0;
	}
	
	update();

	render();
}

void CCore::update()
{
	// 물체들의 좌표나 변경점들을 다 해결하고 픽스된 상태에서 
	// render()에서 다시 그려낸다.

	// 물체를 특정키가 눌렀을 때 포지션 변경
	// 키 입력도 메세지 기반이 아니다. 키 눌린 순간 바로 체크
	// 비동기 키 입출력 함수
	if (GetAsyncKeyState(VK_LEFT) & 0x8000)
	{
		g_obj.m_ptPos.x -= 1;
	}
	
	if (GetAsyncKeyState(VK_RIGHT) & 0x8000)
	{
		g_obj.m_ptPos.x += 1;
	}
}

void CCore::render()
{
	Rectangle(m_hDC,
		g_obj.m_ptPos.x - g_obj.m_ptScale.x / 2,
		g_obj.m_ptPos.y - g_obj.m_ptScale.y / 2,
		g_obj.m_ptPos.x + g_obj.m_ptScale.x / 2,
		g_obj.m_ptPos.y + g_obj.m_ptScale.y / 2);
}

```





