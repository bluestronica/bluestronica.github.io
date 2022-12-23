[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 이전에는 윈도우에 현재 시간을 출력하는 프로그램을 제작했다. 시간은 정상적으로 출력되나,실행하면 바로 뜨지 않고 1초 후에 뜨고, 화면이 깜빡거리는 문제점이 있었다.
- 이 문제를 해결하면서 일정 시간이 되면 알람이 울리는 프로그램을 작성한다.


# 이전 프로그램의 문제점과 해결 방안

### 실행 시 시간이 바로 출력되지 않고 1초 후에 출력된다.
- 원인
  - WM_TIMER 메세지 발생 시점이 실행 후 1초 뒤이기 때문에 발생하는 문제이다.
  - WM_CREATE 메세지에서 SetTimer 함수를 통해 타이머를 설치했지만,
  - 1초 뒤 메세지가 발생하기 때문에 실행 후 1초 후에 시간이 출력되는 것이다.
- 해결
  - 이를 해결하기 위해서 타이머를 설치하고 바로 WM_TIMER 메세지를 발생시킬 필요가 있다.
  - 임의로 메세지를 발생시키기 위해서 SendMessage 함수를 사용한다.
  - **`LRESULT SendMessage(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam);`**
  - 메세지를 발생시킬 윈도우와, 메세지 및 부가 정보를 전달하여 메세지를 발생시킬 수 있다.
  - 문제를 해결하기 위해 WM_TIMER를 보내야 한다.
  - 타이머의 ID는 wParam에 저장되므로 세 번째 매개변수로 타이머의 ID를 넣는다.
  - lParam은 함수를 호출할 경우 함수의 주소인데, 함수는 사용하지 않으므로 0을 넣어서 전달한다.

### 화면이 깜빡거린다.
- 원인
  - 이는, WM_TIMER 메세지를 처리하면서 화면에 시간을 업데이트하는 과정에서 발생하는 문제이다.
  - 전체 화면를 지우고 새로 출력하기 때문에 이 과정에서 화면이 깜빡이는 것처럼 보이는 것이다.
- 해결
  - 이를 해결하기 위해서 시계부분만 업데이트를 해야 한다.
  - 시간을 출력하는 문자열을 업데이트하고 화면에 출력하려고 InvalidateRect 함수를 사용했다.
  - 이 함수의 2번째 매개변수를 NULL로 전체 화면을 무효화한 뒤, 3번째 매개변수가 TRUE 이므로 다시 그리도록 코드를 작성했다.
  - 전체 화면을 무효화하지 않고 시계부분만 무효화하기 위해서 2번째 매겨변수에 시계가 있는 영역 정보를 전달하도록 하겠다.
  
### 경고음 출력하기
- 문제점을 해결하고, 5초가 지날때마다 알림이 울리도록 경고음을 출력하는 기능을 추가해보도록 하겠다.
- 알림을 울리기 위해서 MessageBeep 함수를 사용한다.
- **`BOOL MessageBeep(uType);`**
- 매개변수로 들어가는 uType은 알림의 종류이다. 
- 0을 넣게 되면 일반적인 띵~소리가 난다.
- 다양한 소리를 낼 수 있다.


# 코드로 구현하기

### TowTimer 프로젝트
```c
RESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
    	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	SYSTEMTIME st;
	static TCHAR sTime[128] = _T("");
	static RECT rt = { 100, 100, 400, 120 };

	switch (iMessage)
	{
	case WM_CREATE:
		SetTimer(hWnd, 1, 1000, NULL);
		SetTimer(hWnd, 2, 5000, NULL);
		SendMessage(hWnd, WM_TIMER, 1, 0);
		return 0;
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 100, 100, sTime, _tcslen(sTime));
		EndPaint(hWnd, &ps);
		return 0;
	case WM_TIMER:
		switch (wParam) 
		{
		case 1:
			GetLocalTime(&st);
			_stprintf_s(sTime, 
				_T("지금 시간은 %d:%d:%d입니다."), 
				st.wHour, st.wMinute, st.wSecond);
			InvalidateRect(hWnd, &rt, FALSE);
			break;
		case 2:
			MessageBeep(0);
			break;
		}		
		return 0;
	case WM_DESTROY:
		KillTimer(hWnd, 1);
		KillTimer(hWnd, 2);
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 5초마다 알림을 울리기 위해 2번 ID를 갖는 타이머를 생성했다.
- 타이머를 생성한 뒤, 화면에 바로 시간을 출력하기 우해 `SendMessage(hWnd, WM_TIMER, 1, 0)` 함수를 통해 WM_TIMER 메세지를 발생시켜 윈도우 프로시저에게 전달한다.
- wParam에 아이디 값을 넘겨주고, WM_TIMER를 처리하는 부분에서 switch문을 통해 메세지를 처리하는 모습을 확인할 수 있다.
- 또한, 1번 타이머를 처리하는 과정에서 화면에 변화가 있을을 알리기 위해 사용하는 InvalidateRect 함수의 두 번째 매개변수에 시간을 출력하는 **사각 영역 정보를 전달해 이 부분만 무효화시켜** 전체 화면이 깜빡이는 것을 방지했다.


# 일회용 타이머 만들기
- 웹 사이트나 프로그램을 사용하다 버튼에 마우스를 올리면 도움말이 잠깐 떳다가 사라지는 것을 볼 수 있다. 이렇게 일정 시간동안 메세지를 띄웠다가 사라지게 하는, 마우스 왼쪽 버튼을 누르면 문자열을 출력하고 몇 초 후에 사라지게 하는 프로그램을 만든다.
- 이렇게 문자열을 3초동안 띄웠다가 삭제 할 수 있다.
- 하지만, 문자열이 출력되는 동안 시스템이 정지되어 다른 동작을 할 수 없다. 이렇게 되면 Wind32 API의 장점인 백드라운드 작업을 할 수 없다. 
- 다른 메세지가 발생해도 처리할 수 없을 것이다.
- 타이머를 활용해서 일회용 타이머를 생성해, 문자열을 출력하고 타이머가 울리면 문자열을 지우고 타이머를 삭제하여 위와 똑같은 기능을 구현해 낼 수 있다.
- 문자열이 출력되는 동안 다른 메세지를 처리할 수도 있다. 이렇게 타이머는 다양한 곳에서 활용될 수 있다.


# 코드로 구현하기

### OnceTimer 프로젝트
- 왼쪽 마우스를 클릭하면 텍스트가 출력되었다가 3초 뒤 사라는 프로그램이다.
- 단, 이 때 사용된 타이머는 문자열이 사라지면 함께 사라진다. 이를 새로운 윈도우에 띄우면 다른 웹사이트나 프로그램에서 보았던 도움말처럼 응용할 수 있다.
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	static TCHAR str[128] = _T("");

	switch (iMessage)
	{
	case WM_LBUTTONDOWN:
		lstrcpy(str, _T("왼쪽 버튼을 눌렀습니다."));
		InvalidateRect(hWnd, NULL, TRUE);
		SetTimer(hWnd, 1, 3000, NULL);
		return 0;
	case WM_TIMER:
		KillTimer(hWnd, 1);
		lstrcpy(str, _T(""));
		InvalidateRect(hWnd, NULL, TRUE);
		return 0;
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 10, 10, str, _tcslen(str));
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 이 프로젝트의 포인트는 타이머의 생성 위치와 소멸 위치이다.
- 마우스가 눌리면 타이머가 설치되고, 3초 후에 발생하는 WM_TIMER 메세지에서 해당 타이머를 삭제
- 이렇게 되면 3초 후 문자열이 사라지고 나서 똑같은 메세지가 발생하지 않는다.
- 즉, 3초가 지나도 아무 일도 일어나지 않는다. 
- 이렇게 특정 용도를 위해 사용되는 일회용 타이머를 구현할 수 있다.


