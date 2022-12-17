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

### 변수 선언
- 사용자가 입력한 문자를 저장하기 위한 문자열 배열 str을 선언
- 배열에 문자를 넣기 위한 인덱스 len을 선언
- 문자열에 static으로 선언한 이유
  - 만약, static을 사용하지 않았다면,
  - 메세지를 처리하고 나서 함수가 종료되면 해당 문자열은 지역 변수이기 때문에 메모리에서 없어지게 된다.
  - 하지만, 스페이스바가 입력되기 전까지 입력한 문자를 화면에 출력해야한다.
  - 그래서, static을 사용해 전역 변수의 성격도 뛸 수 있도록 작성한 것이다.
  - 즉, WndProc 함수에서만 접근을 할 수 있지만(지역함수), 함수가 종료되어도 메모리에서 사라지지 않는다.(전역변수)
  - 따라서, WndProc 함수가 종료되어도 입력된 문자를 저장한 채 계속해서 출력할 수 있는 것이다.

### WM_CHAR 메세지
- WinMain에서 TranslateMessage 함수를 통해서 발생한 WM_CHAR 메세지가 들어오면, wParam에 저장된 문자 정보를 통해서 메세지를 처리 할 수 있다. 
- 문자열 첫 번째 인덱스에 0을 넣으므로써, 문자열에 아무것도 없다고 눈속임하는 것이다.
- 다른 문자가 들어오면 **`_tcslen()` 함수를 통해서 문자열의 길이를 확인**한다.
- **`_tcslen()` 함수는 0번째 인덱스부터 NULL 값까지의 길이를 확인**하기 때문에 첫 번째 인데스에 0을 넣으면 아무것도 없다고 눈속임하는 것이 가능한 것이다. 
- 결과적으로, 해당 인덱스에 문자를 넣고 그 다음 인덱스에 0을 넣어서 방금 입력된 문자까지 출력하게 한다.
- 여기서 중요한 것은, 문자 배열의 값만 변경했을 뿐 화면에 변화가 일어나지 않는다는 것이다.
- 이는, 윈도우가 화면에 변화가 없기 때문에 WM_PAINT 메세지를 발생시키지 않는다.
- 이 말은, 변견된 문자열은 화면에 출력하지 않는다는 것이다.
- 따라서, **윈도우에 변화가 있다는 것을 알릴 필요가 있다.**
- **이 때, 사용하는 것이 InvalidateRect 함수이다.**

# InvalidateRect 함수와 무효 영역(Invalid Region)

### 무효 영역(Invalid Region)
- 1번 메모장의 내용이 2번 메모장에 의해 일부 가려져 보이지 않는다. 1번 메모장의 내용을 확인하기 위해 2번 메모장을 이동하면,
- 윈도우는 이를 판단하고 WM_PAINT 메세지를 발생시켜 다시 출력하게 된다.
- 이처럼, 다른 응용 프로그램에 의해 일부 가려져 있는 부분을 무효 영역이라고 한다.

### InvalidateRect 함수
- 위의 소스에서 WM_CHAR 메세지를 처리하는 것처럼 내부적인 문자 입력은 화면에 보여지는 것인지 단순 내부적인 계산, 전송인지 운영체제가 판단할 수 없다.
- 이 때, InvalidateRect 혹은 InvalidateRgn 함수를 사용해서 윈도우의 작업 영역을 무효화하여 운영체제로 하여금 WM_PAINT 메세지를 해당 윈도우로 보내도록 하는 것이다.
- **`BOOL InvalidateRect(HWND hWnd, CONST RECT RECT *lpRect, BOOL bErase);`**
- HWND hWnd : 무효화 시킬 윈도우(윈도의 핸들)
- CONST RECT RECT *lpRect : 무효화할 영역 지적(NULL이면 전체 영역 무효화)
- BOOL bErase : TRUE이면 지우고 다시 그리고, FALSE이면 지우지 않고 덮어쓴다.
- InvalidateRect 함수를 통해서 화면의 변화가 있다고 운영체제에게 알린 뒤, WM_PAINT 메세지가 발생해 화면에 문자열을 출력하는 것이다.

# 방향키를 통해서 이동하기
- 대부분의 게임은 화살표 방향키를 통해서 상,하,좌,우로 이동하곤 한다. 
- KeyDown 프로젝트는 화면에 네모를 출력해 방향키를 통해서 이동시키는 것을 진행한다.

### 가상 키 코드(Virtual Key Code)

| 가상키 코드 | 값 | 키 |
|:---|:---|:---|
|VK_BACK| 08 |Backspace|
|VK_TAB| 09 |Tab|
|VK_RETURN| 0D |Enter|
|VK_SHIFT| 10 |Shift|
|VK_CONTROL| 11 |Ctrl|
|VK_MENU| 12 |Alt|
|VK_PAUSE| 13 |Pause|
|VK_CAPITAL| 14 |Caps Lock|
|VK_ESCAPE| 1B |Esc|
|VK_SPACE| 20 |Space|
|VK_LEFT| 25 |좌측 이동키|


















