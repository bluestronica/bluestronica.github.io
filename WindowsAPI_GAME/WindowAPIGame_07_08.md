# Singleton Pattern

### 동적할당을 이용해서 싱글톤 만드는 방식
```c++
class CCore
{
private:
	static CCore* g_pInst;

public:
	// 정적 멤버함수
	static CCore* GetInstance()
	{
		// 최초 호출 된 경우
		if (nullptr == g_pInst)
		{
			g_pInst = new CCore;
		}

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


















# Core Class





















