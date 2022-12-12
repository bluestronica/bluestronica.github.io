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
}
```

### WinMain 함수의 역할
- WinMain 함수는 다음과 같이 5개의 기능을 한다.
  - **`WndClass 정의 >> WndClass 등록 >> 윈도우 생성 >> 윈도우 출력 >> 메세지 루프`**



























