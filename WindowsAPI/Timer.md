[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- Win32 API를 활용해 입, 출력을 하는 것을 다루었다. 텍스트, 도형 등을 출력하고, 마우스 클릭을 통해 그림을 그리는 프로그램을 작성했다. 코드는 순차적으로 진행했기 때문에, 이러한 동작들은 개발자가 의도한 순서에 의해 동작하는 코드들이었다. 이번에는 **타이머를 통해 일정 시간마다 메세지 혹은 함수를 호출해 동작하는 것**에 대해 다루겠다.

# 타이머 설치와 제거
- Win32 API의 장점 중 하나는 백그라운드 작업을 지원한다는 것이다. 백그라운드 작업을 구현하는 방법 중 하나는 타이머를 활용하는 것이다. 타이머를 설치해 일정 시간마다 메세지 혹은 함수를 호출해 작업을 제어하도록 하는 것이다. **타이머는 SetTimer 함수를 통해 설치**할 수 있다.
- **`UINT SetTimer(HWND hWnd, UINT nIDEvent, UINT uElapse, TIMERPROC lpTimerFunc)`**
  - HWND hWnd
    - 타이머가 발생하는 윈도우의 핸들
  - UINT nIDEvent
    - 해당 타이머의 ID
    - 프로그램에 타이머가 여러 개 존재할 수 있기 때문에 구분을 위해 타이머 ID를 설정한다.
    - 개발자가 직접 타이머 ID를 부여한다.
  - UINT uElapse
    - 타이머의 주기 (n/1000초)    
  - TIMERPROC lpTimerFunc
    - 타이머가 발생하면 호출될 전용 함수이다.
    - 함수를 호출할 수 있지만, NULL 값을 넣을 경우 WM_TIMER 메세지를 발생시킨다.
- **`SetTimer(hWnd, 1, 1000, NULL);`**
  - 초마다 WM_TIMER 메세지를 발생 -> ID값 1
- 사용이 끝난 타이머는 반드시 제거해야 한다. 타이머를 제거할 때는 KillTimer 함수를 사용한다.
  - **`KillTimer(HWND, TimerID)`**  

### SetTime() 함수와 KillTimer() 함수의 위치?
- WM_CREATE 메세지
  - WM_CRATE 메세지는 윈도우가 처음 생성될 때 발생하는 메세지이다. 
  - 해당 메세지에서 실행에 필요한 메모리 할당, 전역 변수 초기값 등 프로그램 시작 시 꼭 한 번만 초기화해야 하는 것들을 처리 할 수 있다.
- WM_DESTORY
  - 프로그램이 종료할 때 발생하는 메세지이다.
  - WM_DESTROY 메세지에서 생성된 타이머를 KillTimer 함수를 통해 제거할 수 있다.

# 타이머를 활용해 시계 만들기

### MY_TIMER 프로젝트
- 화면에 현재 시간을 출력하는 프로젝트이다.
- 현재 시간을 구하기 위한 SYSTEMTIME 구조체와 GetLocalTime 함수

| 멤버 변수 이름 | 내용 |
|:---|:---|
| wYear | 현재 연도를 지정 |
| wMonth | 현재 월을 지정 |
| wDayOfWeek | 현재 요일을 지정(일=0, 월=1, 화=2 ... |
| wDay | 현재 날짜를 지정 |
| wHour | 현재 시간을 지정 |
| wMinute | 현재 분을 지정 |
| wSecond | 현재 초를 지정 |
| wMilliseconds | 현재 밀리초를 지 |

```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
    	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;          // 출력을 위한 Device Context의 핸들
	PAINTSTRUCT ps;   // 출력을 위한 구조체
	SYSTEMTIME st;    // 시간 정보를 저장하기 위한 구조체
	static TCHAR sTime[128] = _T("");    // 시간 정보를 출력하기 위한 문자열
	static RECT rt = { 100, 100, 400, 120 };   // 문자열을 출력할 사각 영역
	                                           // (사각 영역 내부에 출력)

	switch (iMessage)
	{
	case WM_CREATE:
		SetTimer(hWnd, 1, 1000, NULL);
		SendMessage(hWnd, WM_TIMER, 1, 0);
		return 0;
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 100, 100, sTime, _tcslen(sTime));
		EndPaint(hWnd, &ps);
		return 0;
	case WM_TIMER:
		GetLocalTime(&st);
		_stprintf_s(sTime, _T("지금 시간은 %d:%d:%d입니다."), 
			    st.wHour, st.wMinute, st.wSecond);
		InvalidateRect(hWnd, &rt, FALSE);
		return 0;
	case WM_DESTROY:
		KillTimer(hWnd, 1);
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

### 메세지 처리
- WM_CREATE
  - 윈도우가 실행되면, 먼저 WM_CREATE 메세지가 실행된다. 
  - WM_CREATE 메세지가 발생하면 SetTimer 함수를 통해 1번 ID를 가진 타이머를 설치한다.
  - SetTimer 함수의 마지막 매개변수가 NULL이기 때문에, 
  - 이 타이머는 1초에 한 번씩 WM_TIMER 메세지를 발생 시킨다.
- WM_TIMER
  - 설치된 타이머가 1개밖에 없으므로 따로 switch문을 사용하지 않고 바로 코드를 작성한다.
  - 1초마다 WM_TIMER 메세지가 발생할 때마다, 현재 시간을 업데이트한다.
  - GetLocalTime 함수를 통해 현재 시간을 구조체에 업데이트하고, 해당 구조체의 정보를 문자열로 나타내 sTimer에 저장한다.
  - 문자열을 저장하는 함수 `_stprintf_s`가 사용된다.
  - 시간이 흘렀으니, 화면에 시간을 출력해야 한다. 시간을 업데이트 하는 것은 내부적인 동작이므로, 
  - 윈도우는 이 변화를 감지하지 못한다. 따라서 InvalidateRect 함수를 통해 화면을 업데이트한다.
- WM_PAINT
  - InvalidateRect 함수로 인해 WM_PAINT 메세지가 발생한다.
  - 두 번째 인자가 NULL이므로, 모든 화면이 지워졌다가 다시 그려지게 된다.
  - TextOut 함수를 통해 화면에 업데이트된 시간 정보를 출력한다.
- WM_DESTORY
 - 윈도우가 종료되면 WM_DESTORY 메세지가 발생한다.
 - 반대로, 개발자가 WM_DESTORY 메세지를 발생시켜 프로그램을 종료할 수도 있다.
 - 혹은 사용자가 우측 상단의 X표시를 눌렀을 때도 해당 메시지가 발생한다.
 - 여기서 만들었던 타이머는 KillTimer 함수에 의해 삭제된다.

### 문제점
- 프로그램이 시작하고 1초가 지나서야 화면에 시간이 출력된다.
  - 타이머가 설치되고 1초가 지나야 WM_TIMER 메세지가 발생하기 때문에, 프로그램이 시작하고 1초가 지나야 화면에 시간이 출력되는 문제점이 있다.
- 화면이 깜빡이는 현상이 발생한다.
  - InvalidateRect 함수 사용 시, 두 번째 매개변수를 NULL로 설정했기 때문에, 전체 화면이 지웠다가 다시 그려진다.
  - 따라서, 화면이 깜빡거리거나 부자연스러운 현상이 발생한다.

# 정리
- 타이머를 통해 사용자의 동작과 상관없이, 백그라운드에서 동작하는 프로그램을 작성했다.
- 타이머는 SetTimer 함수를 통해 설치할 수 있다. 
- 설치된 타이머는 반드시 KillTimer 함수를 통해 삭제되어야 한다.



