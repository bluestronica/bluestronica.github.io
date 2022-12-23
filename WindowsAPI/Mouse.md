[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 키보드 입력을 통해 문자열을 화면에 출력하는 프로젝트를 작성했다.
- 이 과정에서 몇몇 변수를 static으로 선언하는 이유와 가상키 코드에 대해 알아봤다.

# 마우스 입력시 발생하는 메세지
- 마우스를 클릭하게 되면 해당 메세지가 발생하게 된다. 
- 마우스를 눌렀을 때, 마우스를 이동할 때, 더블클릭 할 때, 눌렀다가 뗏을 때 각각의 메세지가 발생한다. 
- 왼쪽 버튼을 눌렀는지, 오른쪽 버튼을 눌렀는지에 따라 각각의 메세지가 발생한다. 
- 마우스와 관련된 메세지는 다음과 같다.

| 버튼 | 누름 | 놓음 | 더블 클릭 |
|:---|:---|:---|:---|
| 왼쪽 버튼 | WM_LBUTTONDOWN | WM_LBUTTONUP | WM_LBUTTONDBLCLK |
| 오른쪽 버튼 | WM_RBUTTONDOWN | WM_RBUTTONUP | WM_RBUTTONDBLCLK |
| 중앙 버튼 | WM_MBUTTONDOWN | WM_MBUTTONUP | WM_MBUTTONDBLCLK |

- 메세지 루프에서 WM_LBUTTONDOWN 메세지 발생하면 윈도우 프로시저로 메세지가 전달된다.
- 이 때, 마우스가 클릭된 위치나 조합키의 상태(Shif, Ctrl 등)와 같은 부가 정보가 메세지와 함께 윈도우 프로시저로 전달된다.
- **`LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)`**
- 부가 정보는 WPARAM, LPARAM에 담겨서 전달된다.

### LPARAM lParam
- 이 구조체는 WORD형 구조체이다. 4byte로 이루어져 있고, 
- 상위 2byte는 HIWORD, 하위 2byte는 LOWORD라고 한다.
![img](Img/lparam.png)
- 위 그림처럼, 마우스의 위치 정보는 LPARAM의 HIWORD에 Y좌표, LOWORD에 X좌표가 담겨 윈도우 프로시저로 전달된다. 실제로 사용할 땐 (LOWORD(lParam), HIWORD(lParam))과 같이 사용할 수 있다.

### WPARAM wParam
- 마우스 버튼의 상태와 키보드 조합키의 상태가 전달된다.
- 상태 값들의 비트 연산을 통해 여러 개의 조합키가 눌려도 윈도우가 이를 인식하고 알맞은 처리를 할 수 있는 것이다.

| 값 | 내용 |
|:---|:---|
| MK_CONTROL | Ctrl 키가 눌려있다. |
| MK_LBUTTON | 마우스 왼쪽 버튼이 눌려있다. |
| MK_RBUTTON | 마우스 오른쪽 버튼이 눌려있다. |
| MK_MBUTTON | 마우스 가운데 버튼이 눌려있다. |
| MK_SHIFT | Shift 키가 눌려있다. |


# 자유 곡선 그리기
- 마우스를 클릭해 그림을 그릴 수 있는 프로그램 작성

### Mouse 프로젝트
- 마우스를 클릭하면, 마우스의 위치 정보를 최신화한다.
- 마우스를 움직이면 MoveToEx함수를 통해 최신화한 마우스의 위치 정보로 이동 한 뒤,
- 현재 위치로 LineTo함수를 통해 선을 그린다.
- 더블클릭 메세지 설정(CS_DBLCLKS)
  - **WindMain 함수에서 윈도우를 생성할 때, 더블클릭을 인식할 수 있도록 스타일을 설정해야한다.**
  - 해당 설정을 안하면, 아무리 빨리 클릭을 하더라도 LBUTTONDOWN, RBUTTONDOWN 메세지가 많이 발생할 뿐, 더블클릭 메세지가 발생하지 않는다.
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

	WndClass.cbClsExtra = 0;
	WndClass.cbWndExtra = 0;
	WndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	WndClass.hCursor = LoadCursor(NULL, IDC_ARROW);
	WndClass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
	WndClass.hInstance = hInstance;
	WndClass.lpfnWndProc = WndProc;
	WndClass.lpszClassName = lpszClass;
	WndClass.lpszMenuName = NULL;
	WndClass.style = CS_HREDRAW | CS_VREDRAW | CS_DBLCLKS;  // 더블클릭 인식!

	RegisterClass(&WndClass);

	hWnd = CreateWindow(lpszClass, lpszClass, WS_OVERLAPPEDWINDOW,
		CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
		NULL, (HMENU)NULL, hInstance, NULL);

	hWndMain = hWnd;
	ShowWindow(hWnd, nCmdShow);

	while (GetMessage(&Message, NULL, 0, 0))
	{
		TranslateMessage(&Message);
		DispatchMessage(&Message);
	}
	return (int)Message.wParam;
}

LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
  	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	static int x;
	static int y;
	static BOOL bNowDraw = FALSE;

	switch (iMessage)
	{
	case WM_LBUTTONDOWN:
			x = LOWORD(lParam);
			y = HIWORD(lParam);
			bNowDraw = TRUE;
			return 0;
	case WM_MOUSEMOVE:
		if (bNowDraw == TRUE)
		{
			hdc = GetDC(hWnd);
			MoveToEx(hdc, x, y, NULL);
			x = LOWORD(lParam);
			y = HIWORD(lParam);
			LineTo(hdc, x, y);
			ReleaseDC(hWnd, hdc);
		}
		return 0;
	case WM_LBUTTONUP:
		bNowDraw = FALSE;
		return 0;
	case WM_LBUTTONDBLCLK:
		InvalidateRect(hWnd, NULL, TRUE);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

### 변수 선언
- 위치 정보를 저장할 x, y와 마우스가 눌려있는 상태인지 확인하기 위한 토글인 bNowDraw 변수가 static으로 선언되어 있다.
- 위치 정보와 토글 정보는 해당 윈도우 프로시저가 종료되고 다음 메세지를 처리하기 위한 윈도우 프로시저가 실행되어도 해당 정보를 기억하고 있어야 하기 때문에 static으로 선언해, 함수가 종료되어도 메모리에서 삭제되지 않도록 하는 것이다.

### WM_LBUTTONDOWN, WM_MOUSEMOVE 메세지
- WM_LBUTTONDOWN
  - 왼쪽 마우스를 클릭하면 lParam에 저장되어 있는 위치 정보를 변수에 저장한다.
  - 그리고, 마우스가 눌렸기 때문에 bNowDraw를 TRUE로 최신화한다.
- WM_MOUSEMOVE
  - 마우스가 눌려있는 것과 상관없이, 마우스가 움직이면 항상 발생한다.
  - 따라서 조건문을 통해 마우스가 눌러 있을 때만 코드를 실행하게 한다.
  - 즉, 마우스가 눌려있는 상태(bNowDraw가 TRUE이면)면 선을 그린다.
- 선을 화면에 출력하기 위해서 Device Context가 필요하다.
- WM_PAINT 메세지가 아니기 때문에, GetDC를 통해 Device Context를 얻는다.
- 그리고, MoveToEX 함수를 통해 기존에 저장되어 있는 위치로 이동한다.
- 그리고 나서 위치 정보를 최신화 한 뒤에 LineTo 함수를 통해 선을 그리는 것이다.
- 출력이 끝나면 ReleaseDC 함수를 통해 Device Context를 종료한다.

### WM_LBUTTONUP, WM_LBUTTONDBLCLK 메세지
- WM_LBUTTONUP
  - 눌려있던 왼쪽 마우스를 떼면 WM_LBUTTONUP 메세지가 발생한다.
  - 누르고 있던 마우스를 뗏으므로 bNowDraw를 FALSE로 변경한다.
- WM_LBUTTONDBLCLK
  - 더블 클릭을 하면 그러져 있던 선들을 앲애야 한다.
  - InvalidateRect 함수를 통해 구현할 수 있다. 
  - 화면의 무효영역을 설정해 일부 영역을 최신화 하는 것이다.
  - InvalidateRect 함수의 세번째 매개변수를 TRUE로 설정하면 기존에 있던 것은 모두 지우는 것이다. 
  - 따라서, 그려져 있던 곡선을 지울 수 있다.
  

