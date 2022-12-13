[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# Windows API의 기본 구조 3
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
	
	while (GetMessage(&Message, NULL, 0, 0))
	{
		TranslateMessage(&Message);
		DispatchMessage(&Message);
	}
	return (int)Message.wParam;
}

LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	switch (iMessage)
	{
	case WM_CREATE:
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

### 윈도우 프로시저(Window Procedure, WndProc 함수)
- WinMain 함수에서 메세지 루프를 통해서 발생한 메세지를 큐에 저장한다. 
- 해당 메세지를 처리하기 위해서 메세지 처리 전용 함수로 전달되어야 한다. 
- 이 때, 메세지 처리 전용 함수는 윈도우 프로시저이다. 
- 윈도우 프로시저는 WinMain 함수와는 별도로 WndProc 함수의 형태로 존재한다.

### 윈도우 프로시저의 특징
- WinMain에서 호출하는 것이 아닌, 윈도우에 의해서 호출된다.
- WinMain내에 존재하는 메세지 루프는 메세지 처리 전용 함수로 전달하는 역할만 한다.
- 윈도우 프로시저는 메세지가 들어오면 호출되며, 메세지에 맞게 내용을 처리한다.
- 콜백 함수(CallBack Function)이다. 
  - 사용자가 호출하는 것이 아닌, 운영체제에 의해 호출되는 함수

### WndProc 함수
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	switch (iMessage)
	{
	case WM_CREATE:
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- WndProc 함수의 인수
  - HWND hWnd : 메세지를 받을 윈도우의 핸들
  - UINT iMessage : 어떤 메세지를 받았는지
  - WPARAM wParam : 메세지에 따른 부가 정보
  - LPARAM lParam : 메세지에 따른 부가 정보
  - 어떤 윈도우에서 처리해야 할 메세지인지, 어떤 메세지를 처리해야 하는지, 메세지에 따른 부가정보에 대한 내용을 담고 있다.
- swithc문을 통해 각 메세지별로 처리
  - WM_CREATE : 윈도우가 생성되면 발생하는 메세지
  - WM_DESTROY : 응용 프로그램의 오른쪽 위에 있는 X표시를 누르게 되면 WM_DESTROY 메세지가 발생되어 윈도우가 종료된다. 
- 윈도우 핸등레 대한 정보는 있지만, 프로그램에 대한 정보가 인수로 없는 이유는 전역변수(g_hInst)를 선언해 프로그램 정보를 어느 함수에서도 접근할 수 있기 때문이다.
- 메세지 처리 순서도(Flow Chart)























