[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# Windows API의 기본 구조 2

```c
#include <Windows.h>
#include <TCHAR.h>

LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);
HINSTANCE g_hInst;
HWND hWndMain;
LPCTSTR lpszClass = _T("Turtle Rabbit");

int APIENTRY WinMain(_In_ HINSTANCE hInstance, 
                     _In_opt_ HINSTANCE hPrevInstance, 
                     _In_ LPSTR lpszCmdParam, 
                     _In_ int nCmdShow)
{
	HWND hWnd;
	MSG Message;
	WNDCLASS WndClass;
	g_hInst = hInstance;
	
	WndClass.cbClsExtra = 0;
	WndClass.cbWndExtra = 0;
	WndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	WndClass.hCursor = LoadCursor(NULL, IDC_ARROW);
	WndClass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
	WndClass.hInstance = hInstance;
	WndClass.lpfnWndProc = WndProc;
	WndClass.lpszClassName = lpszClass;
	WndClass.lpszMenuName = NULL;
	WndClass.style = CS_HREDRAW | CS_VREDRAW;	
	
	RegisterClass(&WndClass);
	
	hWnd = CreateWindow(lpszClass, lpszClass, WS_OVERLAPPEDWINDOW,
		CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
		NULL, (HMENU)NULL, hInstance, NULL);
	
	hWndMain = hWnd;
	ShowWindow(hWnd, nCmdShow);
}
```

### WinMain 함수의 역할
- WinMain 함수는 다음과 같이 5개의 기능을 한다.
  - **`WndClass 정의 >> WndClass 등록 >> 윈도우 생성 >> 윈도우 출력 >> 메세지 루프`**
- **WndClass 정의**
  - WndClass는 윈도우 클래스를 의미한다. 윈도우 기반이 되는 클래스를 윈도우 클래스라고 한다.
  - WndClass 정의는 만들고자 하는 윈도우의 속성을 정의하는 것을 의미한다.
  - 윈도우의 배경색, 아이콘, 스타일 등등 속성을 정의 할 수 있다.
  - 그뿐아니라, 윈도우에 발생하는 메세지는 어떤 함수에서 처리할지, 해당 인스턴스(프로그램)의 핸들은 무엇인지와 같은 정보들을 정의할 수 있다.
- **WndClass 등록**
  - 윈도우의 속성을 정의하면 윈도우 클래스를 등록해야 한다.
  - 정의된 윈도우를 운영체제가 윈도우 클래스를 인지 할 수 있도록 커널에 등록하는 과정을 거쳐야한다.
- **윈도우 생성**
  - 윈도우 클래스가 커널에 등록되면 메모리에 윈도우 클래스를 올릴 수 있다.
  - 메모리에 윈도우를 만드는 것을 "윈도우 생성"이라고 한다.
  - 메모리에 윈도우가 생성되면 이에 대한 핸들값을 알 수 있다.
- **윈도우 출력**
  - 윈도우가 커널에 등록되고 메모리에 생성되면 이제 사용자에게 보여줘야 한다.
  - 시각화(화면에 출력)하는 것도 WinMain 함수의 역할이다.
- **메시지 루프**
  - 화면에 출력되고 사용자가 사용하기 시작하면 여러가지 메세지(이벤트)가 발생한다.
  - 메세지가 언제 들어올지 알 수 없기 때문에 컴퓨터는 메세지 루프를 돌면서 메세지가 발생하는 것을 기다리고 있다.
  - WinMain에 메세지가 발생할 때까지 메세지 루프를 돌다가, 메세지가 발생하면 해당 메세지를 메세지 처리 전용 함수(윈도우 프로시저, WndProc)로 메세지를 전달한다. 메세지 큐에 쌓여 있는 메세지를 꺼내와 WndProc에 전달하게 된다.
  
### WinMain의 파라미터와 변수 선언
```c
int APIENTRY WinMain(_In_ HINSTANCE hInstance, 
                     _In_opt_ HINSTANCE hPrevInstance, 
                     _In_ LPSTR lpszCmdParam, 
                     _In_ int nCmdShow)
{
	HWND hWnd;
	MSG Message;
	WNDCLASS WndClass;
	g_hInst = hInstance;
}
```
- **APIENTRY**
  - APIENTRY는 윈도우 표준 호출 규약인 `_stdcall`을 사용한다는 의미이다.
  - `_stdcall` 의 특징은 파라미터의 처리를 호출된 쪽에서 한다는 것이다.
  - 원래 우리가 함수를 사용할 때, 인수를 포함한 함수를 사용할 때 처리된 값을 넣어서 함수를 호출하게 된다.
  - 반면, `_stdcall` 을 사용하는 WinMain은 발생한 메세지를 전달만하고 메세지에 대한 처리는 메세지 처리 젼용 함수인 WndProc에서 하게 되는 것이다.
- **인수**
  - `_In_` 과 `_In_opt_` 
    - 이것은 Visual Studio가 2019로 넘어오면서 파라미터에 접두사를 추가해서 작성하는 것을 권장하기 때문에 사용하는 것이다. 
- **HINSTANCE hInstance**
  - 프로그램의 인스턴스 핸들이다.
  - 전역으로 선언한 전역변수 g_hInst는 파라미터로 받은 hInstance(인스턴스 핸들) 값을 넘겨 받는다.
  - 인스턴스의 핸들을 WinMain에서 선언해버리면 지역변수로 WinMain에서만 사용할 수 있게된다.
  - 정리하면, 파라미터로 받은 인스턴스의 핸들 값을 전역 변수에 전달해서 다른 함수들이 사용할 수 있도록 하는 것이다.

- **HINSTANCE hPrevInstance**
  - hPrevInstance는 이전에 실행된 프로그램의 인스턴스 핸들이다.
  - 이전에 실행된 프로그램이 없을 경우 NULL을 갖는다. 하지만 지금은 사용되지 않는 파라미터이다.
  - 호환성을 위해서 존재하는 파라미터이므로 신경쓰지 않아도 된다. 따라서 WIN32에서는 항상 NULL 값이다.
- **LPSTR lpszCmdParam**
  - 명령행(도스,터미널)으로 입력된 프로그램 인수이다. 도스의 argv 인수에 해당된다. 마찬가지로 거의 사용되지 않는다. 
- **int nCmdShow**
  - 프로그램이 실행될 형태이다.
  - 최소화, 보통 상태일 때 모양이 전달된다.
- 변수 선언
  - **`HWND hWnd;`**
    - 윈도우의 핸들을 선언한다. 윈도우 핸들 구조체에 관련 정보가 담겨있다.
  - **`MSG Message;`**
    - 메세지 구조체를 선언한다. 메세지에 대한 정보가 구조체에 담경있다.
  - **`WNDCLASS WndClass;`**
    - 윈도우 클래스를 선언한다. 다음이 이 윈도우 클래스를 정의하는 부분이다.
  - **`g_Hinst = Hinstance;`**
    - 파라미터로 받은 인스턴스의 핸들을 전역 변수에 저장하는 것이다.
  
### 윈도우 클래스의 정의
- WNDCLASS 구조체의 멤버변수에 값을 저장하는 모습을 볼 수 있다.
```c
WndClass.cbClsExtra = 0;
WndClass.cbWndExtra = 0;
WndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
WndClass.hCursor = LoadCursor(NULL, IDC_ARROW);
WndClass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
WndClass.hInstance = hInstance;
WndClass.lpfnWndProc = WndProc;
WndClass.lpszClassName = lpszClass;
WndClass.lpszMenuName = NULL;
WndClass.style = CS_HREDRAW | CS_VREDRAW;
```
| 속성 이름 | 의미 |
|:---|:---|
| cbClsExtra / cbWndExtra| 특수한 목적으로 사용하는 윈도우 예약 영역 / 사용하지 않을 때는 0으로 지정 |
| hbrBackground | 윈도우 배경 색상을 정한다. / 배경 색상을 브러쉬로 칠하기 때문에 br이란 접두사가 붙는다. |
| hCursor / hIcon | 윈도우가 사용할 커서와 아이콘을 정한다. |
| hInstance | 해당 윈도우 클래스를 등록하는 프로그램의 번호 |
| lpfnWndProc | 해당 윈도우에서 발생하는 모든 메세지를 처리하는 전용 함수를 지정한다. |
| lpszClassName | 윈도우 클래스의 이름을 정의 |
| lpszMenuName | 만들어질 프로그램의 사용할 메뉴를 지정한다. |
| style | 윈도우의 형태를 지정한다. (or)을 사용하 여러개를 사용한다. |

### 윈도우 클래스 등록
- 위에서 정의한 윈도우 클래스를 커널에 등록하는 과정을 거칠 차례이다.
- **` RegisterClass(&WndClass); `**


### 윈도우 생성
- 등록된 윈도우를 메모리에 생성하는 과정이다.
- 함수의 결과를 미선 선언되어 있던 hWnd(윈도우 핸들)에 저장한다.
```c
hWnd = CreateWindow(lpszClass, lpszClass, WS_OVERLAPPEDWINDOW,
	CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
	NULL, (HMENU)NULL, hInstance, NULL);
```
- CreateWindow 함수의 원형
```c
HWND CreateWindow(lpszClassName, lpszWindowName, dwStyle, 
	x, y, nWidth, nHeight, hWndParent, hMennu, hInst, lpvParam)
```
- lpszClassName
  - 생성하는 윈도우 클래스를 지정하는 문자열이다. 
  - 전역 변수 lpszClass에 클래스 이름(Turtle Rabbit)을 넣었다.
- opszWindowName
  - 생성되는 윈도우의 타이틀 바에 나타나는 문자열
- dwStyle
  - 윈도우 형태를 지정한다.
  - WS_OVERLAPPEDWINDOW는 메모장과 유사한 형태로 가장 무난한 윈도우 스타일이다.
  - 이렇게 윈도우 스타일은 접두사로 `WS_` 가 붙는 모습이다.
- x, y, nWidth, nHeight
  - 생성되는 윈도우의 x, y 좌표와 넓이 높이를 나타낸다.
  - CW_USEREFAULT라는 값은 운영체제가 판단하길 가장 적합한 위치와 크기를 맞춰서 출력한다.
- hWndParent
  - 생성되는 윈도우의 부모 윈도우를 나타낸다.
  - 지금은 메인 윈도우를 생성하고 있기 때문에 부모 윈도우가 존재하지 않아 NULL 값을 전달했다.
  - 하지만, 컨트롤과 같은 부모 윈도우를 가지는 차일드 윈도우를 생성할 때는 부모 윈도우를 넣어주게 된다.
- hMenu
  - 윈도우에서 사용할 메뉴의 핸들을 나타낸다.
  - 메뉴의 핸들 값을 HMENU로 형변환해서 넣어주게 된다. 
  - 예제는 NULL 값이 들어가는데, 이는 윈도우 클래스의 속성을 정의할 때 정한 메뉴를 사용하는 것이다.
- hInst
  - 윈도우를 만들어주는 주체인 프로그램의 핸들을 지정한다. 
  - WinMain 함수의 매개변수 받은 hInstance를 넣는다.
- lpvParam
  - CREATESTRUCT 라는 구조체의 주소로, 특별한 목적이 있을 때만 사용한다.
  - 보통은 NULL 값을 사용한다.

### 윈도우 정보 전달 및 윈도우 출력
```c
hWndMain = hWnd;
ShowWindow(hWnd, nCmdShow);
```
- 생성된 윈도우의 정보를 미리 선언한 전역변수 hWnd에 전달해줘야한다.
  - 메인 윈도우의 핸들을 전역변수로 선언한 이유는 다른 API 함수들도 윈도우 핸들의 정보가 필요한 경우가 있기 때문이다.
- 전역 변수에 윈도우 핸들을 전달하고 나면 화면에 출력할 차례이다.
  - ShowWindow 함수에는 두 개의 인수가 들어가는데, 
  - 첫 번째 인수는 hWnd로, 출력될 윈도우의 핸들이다.
    - CreateWindow를 통해서 생성된 윈도우의 핸들을 넘겨주면 된다.
  - 두 번째 인수는 nCmdShow이다.
    - WinMain의 네 번째 파리미터로 받은 이 변수는 윈도우를 화면에 출력하는 방법을 지정하는 역할을 한다. 
    - 다양한 매크로 상수들이 정의되어 있다.
      - SW_HIDE(윈도우를 숨김)
      - SW_SHOW(윈도우를 활성화해 보여줌)
      - SW_MINIMIZE(윈도우 최소화 및 활성화하지 않음)
    
### 메세지 루프

















