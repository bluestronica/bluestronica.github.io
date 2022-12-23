[bluestronica.github.io](https://bluestronica.github.io/)

### [Windows Data Types](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Windows_Data_Types.md)

### [Handle](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Handle.md)

### [Windows API의 기본 구조 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Basic_Structures.md)
- 헤더(header) : `<windows.h>` 와 `<TCHAR.h>`
- WndProc(윈도우 프로시저) 함수
- 전역 변수 선언

### [Windows API의 기본 구조 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Basic_Structures2.md)
- WinMain 함수의 역할
- WinMain의 파라미터와 변수 선언
- 윈도우 클래스(WNDCLASS)의 정의
- 윈도우 클래스 등록
- 윈도우 생성
- 윈도우 정보 전달 및 윈도우 출력
- 메세지 루프

### [Windows API의 기본 구조 3](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Basic_Structures3.md)
- 윈도우 프로시저(Window Procedure, WndProc 함수)
- 윈도우 프로시저의 특징
- WndProc 함수

### [Device Context / 문자열 출력 / 문자열 정렬](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/DC_String.md)
- Device Context(DC)란?
- Device Context가 필요한 이유?
- 문자열 출력하기
- TextOut 프로젝트 1
- 윈도우 프로시저에서 사용할 변수 선언
- WM_LBUTTONDOWN 메세지
- WM_DESTROY 메세지
- TextOut 프로젝트 2
- WM_PAINT 메세지
- BeginPaint() VS GetDC()
- BeginPaint()는 정적(Static) 출력, GetDC()는 동적(Dynamic) 출력
- PAINTSTRUCT 구조체
- 문자열 정렬
- TextOut 프로젝트 3

### [긴 문자열 출력 / 도형 / 메세지박스 출력](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/DrawText_GraphOut_MessageBox.md)
- 긴 문자열 출력
- DrawText 프로젝트
- 윈도우 프로시저에서 사용할 변수 선언
- WM_PAINT 메세지
- 다양한 그래픽(도형) 출력
- Win32 API는 픽셀 하나부터, 선, 원, 사각형 등 다양한 도형을 그릴 수 있는 함수를 지원한다.
- GraphOut 프로젝트
- 메세지 박스(Message Box) 출력
- WM_LBUTTON 메세지

### [키보드 입력 / 사용자가 입력한 문자 출력](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/KeyBoard.md)
- 개요
- Wind32 API 입력
- 포커스와 컨트롤
- 키보드 입력
- key 프로젝트
- 변수 선언
- WM_CHAR 메세지
- InvalidateRect 함수와 무효 영역(Invalid Region)
- 무효 영역(Invalid Region)
- InvalidateRect 함수
- 방향키를 통해서 이동
- 가상 키 코드(Virtual Key Code)
- KeyDown 프로젝트

### [마우스 입력](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Mouse.md)
- 개요
- 마우스 입력시 발생하는 메세지
- LPARAM lParam
- WPARAM wParam
- 자유 곡선 그리기
- Mouse 프로젝트
- 변수 선언
- WM_LBUTTONDOWN, WM_MOUSEMOVE 메세지
- WM_LBUTTONUP, WM_LBUTTONDBLCLK 메세지

### [타이머를 활용해 시계 만들기 1](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Timer.md)
- 개요
- 타이머 설치와 제거
- SetTime() 함수와 KillTimer() 함수의 위치?
- 타이머를 활용해 시계 만들기
- MY_TIMER 프로젝트
- 메세지 처리
- 문제점
- 정리 

### [타이머를 활용해 시계 만들기 2](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Timer2.md)

### [백그라운드 작업과 콜백 함수](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/BackgroundTask_CallbackFunction.md)










