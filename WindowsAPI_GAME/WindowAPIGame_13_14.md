# CKeyMgr

### 키와 프레임 동기화
#### 동일 프레임 내에서 같은 키에 대해 동일한 이벤트를 가져간다.
- 한 프레임에서 발생한 일들은 키 입력에 대해서 동일하게 적용 받아야 한다.
  - 한 프레임 시간(DeltaTime)이 흐르는 시간동안 컴퓨터가 처리한 일들 뿐이지, 그 시간 동안 작업 순서가 먼저라고 해서 델타타임 시간안에서 먼저 일어난 일이라고 보지 않는다. 개별적 사건으로 보지 않는다.
  - 한 프레임 동안 일어난 일들을 다 처리하고 그 결과를 보여주는 것이기 때문에 그 시간안에 일어난 일들은 모두 동일하게 적용 받아야 한다.
- 일괄적으로 각 키들의 상태 값을 체크해 놓고 키매니저를 통해서 각키의 상태를 얻어서 같은 프레임 안에서는 동일한 키에 대해서는 동일한 상태를 세팅 받을 수 있게 한다.
- 그래서 해당 프레임이 시작하는 초입 부분에 키 업데이트를 해 두고 이 때 각 키들의 상태를 정해놓고 이번 프레임 안에서는 그걸 모두 동일하게 적용시킨다.


### 키 입력 이벤트 처리
#### 윈도우에서 제공해주는 기본 함수로는 구체적인 키 이벤트 정의가 제대로 잘 안되어 있다.
- **GetAsyncKeyState**
  - `SHORT GetAsyncKeyState(int vKey)`

  - | 반환값 | 설명 |
    |:---|:---|
    |0 (0x0000)|이전에 누른 적이 없고 호출 시점에서 안눌린 상태|
    |0x8000|이전에 누른 적이 없고 호출 시점에서 눌린 상태|
    |0x8001|이전에 누른 적이 있고 호출 시점에서 눌린 상태|
    |1 (0x0001)|이전에 누른 적이 있고 호출 시점에서 안눌린 상태|
```c++
if(GetAsyncKeyState(VK_LEFT) & 0x8000) -> 지금 키가 눌림
if(GetAsyncKeyState(VK_LEFT) & 0x0001) -> 이전과 지금 사이에 키가 눌림 
if(GetAsyncKeyState(VK_LEFT)) -> 둘다 가능
```

#### 프레임 기준으로 키 입력 상태 나타내기
- 이전 프레임과 현재 프레임을 기준으로 키 상태 나타내기
  - NONE : 눌리지 않은 상태
    - 이전 프레임에 눌리지 않았고, 현재 프레임에도 눌리지 않은 상태
  - TAP : 막 누른 상태
    - 이전 프레임에 눌리지 않았고, 현재 프레임에 막 누른 상태
  - HOLD : 누르고 있는 상태
    - 이전 프레임에 눌려있었고, 현재 프레임에도 눌려있는 상태
  - AWAY : 막 뗀 상태
    - 이전 프레임에는 눌러있었고, 현재 프레임에는 눌려있지 않은 상태
- 이전 프레임과 현재 프레임간의 키 상태 모두 체크

#### 내가 설정한 키와 실제 윈도우 가상 키 값을 매칭
- 배열을 만들어 KEY enum 값을 똑같이 매칭한다.

### 윈도우 포커싱
#### 윈도우에 포커싱이 벗어나면 하고 있던 작업이 정상적으로 마무리가 되도록 키 상태를 순차적으로 변화 시켜야 한다.
- **`TAP -> AWAY -> NONE`**
  - 현재 프레임에 눌러져있을 상태를 현재 프레임에는 눌려있지 않은 상태로 변경
  - 그리고 눌리지 않은 상태로 마무리
- **`HOLD -> AWAY -> NONE`**
  - 현재 프레임에 눌러져있을 상태를 현재 프레임에는 눌려있지 않은 상태로 변경
  - 그리고 눌리지 않은 상태로 마무리
- **`AWAY -> NONE`**
- **`NONE -> NONE`**
  
#### 윈도우 포커싱 알아내기
- **GetFocus**
  - 현재 포커싱 되어있는 윈도우의 핸들 값을 알려준다.
  - 어떠한 윈도우도 포커싱이 되어있지 않으면 0, nullptr로 지칭할 수 있다.
- 윈도 포커싱일 때  
- 윈도우 포커싱 해제상태

### CKeyMgr.h
```c++
enum class KEY_STATE
{
	NONE,	// 이전 프레임에 눌리지 않았고, 현재 프레임에도 눌리지 않은 상태
	TAP,	// 막 누른 시점
	HOLD,	// 누르고 있는
	AWAY,	// 막 뗀 시점 	
};

enum class KEY
{
	LEFT,
	RIGHT,
	UP,
	DOWN,

	Q,
	W,
	E,
	R,
	T,
	Y,
	U,
	I,
	O,
	P,
	A,
	S,
	D,
	F,
	G,
	Z,
	X,
	C,
	V,
	B,

	ALT,
	CTRL,
	LSHIFT,
	SPACE,
	ENTER,
	ESC,

	LAST,
};

struct tKeyInfo
{
	//KEY	eKey; 		  // 벡터 내에 인덱스가 곧 키 값이 된다. 
				  //그래서 존재 의미가 없어 삭제
	KEY_STATE    eSate;	  // 키의 상태 값 
	bool	     bPrevPush;   // 이전 프레임에서 이 키가 눌렸는지 안 눌렸는지
};

class CKeyMgr
{
	SINGLE(CKeyMgr);

private:
	vector<tKeyInfo> mvecKey;

public:
	void init();
	void update();

public:
	KEY_STATE GetKeyState(KEY eKey) { return mvecKey[(int)eKey].eSate; }
};
```

### CKeyMgr.cpp
```c++
int garrVK[(int)KEY::LAST] =
{
	VK_LEFT,
	VK_RIGHT,
	VK_UP,
	VK_DOWN,
	'Q',
	'W',
	'E',
	'R',
	'T',
	'Y',
	'U',
	'I',
	'O',
	'P',
	'A',
	'S',
	'D',
	'F',
	'G',
	'Z',
	'X',
	'C',
	'V',
	'B',
	VK_MENU,
	VK_CONTROL,
	VK_LSHIFT,
	VK_SPACE,
	VK_RETURN,
	VK_ESCAPE,
	//VK_LAST, 끝을 알리는 용도니깐 넣을 필요 없음
};


CKeyMgr::CKeyMgr()
{}
CKeyMgr::~CKeyMgr()
{}

void CKeyMgr::init()
{
	// 따라서 초기화 할때 벡터안에 키 정보를 채워줘야 한다.
	// 접근 : mvecKey[(int)KEY::LEFT].eSate;
	// 접근 : mvecKey[(int)KEY::LEFT].bPrev;

	for (int i = 0; i < (int)KEY::LAST; ++i)
	{
		mvecKey.push_back(tKeyInfo{ KEY_STATE::NONE, false });
	}	
}

void CKeyMgr::update()
{	
	// 윈도우 포커싱 알아내기
	// HWND hMainWnd = CCore::GetInst()->GetMainHwnd();
	HWND hWnd = GetFocus(); // 현재 포커싱 되어있는 윈도우 핸들 값을 알려준다. 

	// 윈도우 포커싱 중일 때 키 이벤트 동작
	if (nullptr != hWnd)
	{
		// 매번 업데이트 해서 모든 키들에 대해서 상태값 체크
		for (int i = 0; i < (int)KEY::LAST; ++i)
		{
			// 키가 눌려있다.
			if (GetAsyncKeyState(garrVK[i]) && 0x8000)
			{
				if (mvecKey[i].bPrevPush)
				{
					// 이전에도 눌려있었다.
					mvecKey[i].eSate = KEY_STATE::HOLD;
				}
				else
				{
					// 이전에는 눌려있지 않았다.
					mvecKey[i].eSate = KEY_STATE::TAP;
				}

				mvecKey[i].bPrevPush = true;
			}
			// 키가 안눌려있다.
			else
			{
				if (mvecKey[i].bPrevPush)
				{
					// 이전에 눌려있었다.
					mvecKey[i].eSate = KEY_STATE::AWAY;
				}
				else
				{
					// 이전에도 안눌려있었다.
					mvecKey[i].eSate = KEY_STATE::NONE;
				}

				mvecKey[i].bPrevPush = false;
			}

		}
	}
	// nullptr == hWnd , 윈도우 포커싱 해제상태
	else
	{
		for (int i = 0; i < (int)KEY::LAST; ++i)
		{
			mvecKey[i].bPrevPush = false;

			if (KEY_STATE::TAP == mvecKey[i].eSate 
				|| KEY_STATE::HOLD == mvecKey[i].eSate)
			{
				mvecKey[i].eSate = KEY_STATE::AWAY;
			}
			else if (KEY_STATE::AWAY == mvecKey[i].eSate)
			{
				mvecKey[i].eSate = KEY_STATE::NONE;
			}
		}
	}
}
```

