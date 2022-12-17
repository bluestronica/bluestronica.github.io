[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 화면에 텍스트를 출력하는 프로젝트를 진행하면서 TextOut, DrawText 함수를 다뤘다.
- 화면에 출력하기 위한 Device Context에 대한 개념과 GetDC, BeginPaint 함수의 차이점에 대해 알아보았다.
- 키보드를 통한 입력에 대해 알아보자!

# Win32 API의 입력
- Win32 API의 장점 중 하나는, 멀티 태스킹 환경을 지원한다는 것이다. 
- 하나의 프로그램이 입력을 받기 위해 대기한다고 해서, 시스템이 멈추면 안된다. 입력을 기다린다고 다른 것을 못하면 멀티 태스킹이 아니다. 
- 따라서, 입력이 들어오면 포커스(입력 초점)을 가진 프로그램에 키보드 메세지를 보내게 되고, 
- 프로그램은 키보드 메세지를 받아 입력을 처리하게 된다. 
- 다시 말하면, 포커스를 가진 컨트롤만 키보드 입력을 받을 수 있고, 
- 이와 관련한 메세지도 포커스를 가진 컨트롤에게만 전달되는 것입니다.

### 포커스와 컨트롤
- 포커스 : 입력 초점
  - 입력 초점이라 함은, 우리가 메모장에 글을 쓸 때, 깜빡거리는 커서 즉, 다음 입력이 들어올 곳을 의미한다.
- 컨트롤 : 인터페이스를 통해 사용자로부터 명령과 입력을 받아들이고 출력 결과를 보여주는 과정에서의 입출력 도구 
  - 입출력 도구라 함음, 메세지 박스, 버튼 등과 같이 사용자와 상호작용하며 입출력을 해주는 것을 의미한다.
  - 즉, 사용자의 입력(저장 버튼 누름)을 받아서 프로그램에 전달하고, 메세지를 처리(내용을 저장)하는 것이 컨트롤이다.
  
# 키보드 입력
- WinMain 함수에서 메세지 루프를 돌고 있다가, 사용자가 키보드 문자를 입력하면
- 발생한 메세지를 WndProc 함수로 전달하게 된다.
```c
while(GetMessage(&Message, NULL, 0, 0))
{
	TranslateMessage(&Message);
	DispatchMessage(&Message);
}
```
- 문자를 입력할 때, 확인해야 할 함수는 TranslateMessage 함수다.
- 메세지 루프에서 입력된 문자 정보를 알아내기 위해 필요하다.
- 사용자가 키보드로 입력을 하면, WM_KEYDOWN 메세지가 발생한다.
  - 정확히는 키를 눌렀을 때 WM_KEYDOWN, 키를 떼면 WM_KEYUP이 발생한다.
- 이는, 키보드가 눌렸다는 메세지 일 뿐, 사용자가 어떤 버튼을 눌렸는지 알 수 없다.
- 이때, **TranslateMessage 함수를 통해 문자 정보를 확인해 WM_CHAR 메세지로 변환해서**
- **DispatchMessage 함수를 통해서 윈도우 프로시저(WndProc)로 전달하게 되는 것이다.**
```C
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
```
- HWND hWnd : 입력이 발생한 윈도우
- UINT iMessage : 전달되는 메세지(여기서는 WM_CHAR)
- WPARAM wParam : 입력 받은 문자 정보(어떤 키를 입력했는지)
- LPARAM lParam : 부가 정보(키 반복 횟수, 이전 키 상태 등)
- **TranslateMessage 함수에서 WM_CHAR 메세지를 발생시키면 wParam에 어떤 키를 눌렀는지에 대한 정보를 전달하는 것이다.** 이 메세지를 프로시저 내 switch문에서 처리하면 된다.

### Key 프로젝트
- 사용작 문자를 입력하면 화면에 출력하고, 스페이스 바를 입력하면 다 지워지는 프로그램이다.
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
  	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	static TCHAR str[256] = _T("");
	int len;

	switch (iMessage)
	{
	case WM_CHAR:
		if (wParam == 32)
		{
			str[0] = 0;
		}
		else
		{
			len = _tcslen(str);
			str[len] = wParam;
			str[len + 1] = 0;
		}
		InvalidateRect(hWnd, NULL, TRUE);
		return 0;
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 50, 50, str, _tcslen(str));
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

























