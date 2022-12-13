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


























