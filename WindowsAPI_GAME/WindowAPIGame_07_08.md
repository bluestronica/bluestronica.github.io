# Singleton Pattern

### 동적할당을 이용해서 싱글톤 만드는 방식
- 개체 주소는 데이터 섹션에 저장
- 개체는 힙 메모리에 할당
- 개체 주소의 유무를 통해 개체의 개수를 통제한다.
- 정적 맴버함수로 바로 접근해서 개체를 생성
  - 개체(힙)와 정적 멤버함수(데이터 섹션)는 하나로 묶여있는 데이터 덩어리가 아니라 서로 저장 위치가 다른 별개의 데이터 덩어리다.
```c++
class CCore
{
private:
	static CCore* g_pInst;  // 데이터형 메모리에 개제 주소 저장하기 위한 정적변수

public:
	// 정적 멤버함수 : 개체를 바로 생성하지 않고 정적 멤버함수에 바로 접근해서 개체를 생성
	static CCore* GetInstance()
	{
		// 최초 호출 된 경우
		if (nullptr == g_pInst)
		{
			g_pInst = new CCore;  // 개체 주소를 데이터형 메모리에 저장
		}                             // 개체는 동적할당(힙) 되어 있다.

		return g_pInst;
	}

	static void Release()
	{
		if (nullptr != g_pInst)
		{
			delete g_pInst;
			g_pInst = nullptr;
		}
	}

private:
	CCore();
	~CCore();
};
```

### 싱글톤으로 만들 유일한 **개체를 데이터형 메모리에 올리는 방식**
- 해제를 신경쓰지 않아도 된다. 프로그램이 종료될까지 데이터가 남는다.
- static은 데이터형 메모리에 저장, core 개체를 데이터형에 만든다.
```c++
class CCore
{
public:
	static CCore* GetInst()
	{
		static CCore mgr;  // 개체를 데이터형 메모리에 저장
		return &mgr;       // 데이터형 메모리에 저장된 개체 주소를 전달
	}

private:
	HWND	m_hWnd;		// 메인 윈도우 핸들
	POINT	m_ptResolution; // 메인 윈도우 해상도

public:
	int init(HWND _hWnd, POINT _ptResolution);
	void progress();

private:
	CCore();
	~CCore();
};
```









# Core Class





















