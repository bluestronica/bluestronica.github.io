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
- 개요
- 이번 프로그램의 문제점과 해결 방안
- 실행 시 시간이 바로 출력되지 않고 1초 후에 출력된다.
- 화면이 깜빡거린다.
- 경고음 출력하기
- 코드로 구현하기
- TowTimer 프로젝트
- 일회용 타이머 만들기
- 코드로 구현하기
- OnceTimer 프로젝트


### [백그라운드 작업과 콜백 함수](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/BackgroundTask_CallbackFunction.md)
- 개요
- 백그라운드 작업
- RandGrp 프로젝트
- 반복문에서 제어권을 독점하고 있는 올바르지 못한 코드
- 제어권을 독점하지 않고, 타이머를 활용해서 백드라운드 작업을 하는 프로그램의 윈도우 프로시저
- 콜백함수
- Callback 프로젝트

### [작업 영역 얻기](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Client_Area.md)
- 개요
- 작업 영역(Client Area)
- GetClientRect 함수
- GetClientRect 함수를 통해 작업 영역 얻기
- WM_CREATE
- WM_SIZE
- WM_PAINT

### [GDI와 스톡 오브젝트(Stock Object)](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/GDI_StockObject.md)
- GDI, GDI 오브젝트, DC의 개념
- 스톡 오브젝트(Stock Object)
- 스톡 오브젝트 활용과 색상
- GdiObject 프로젝트
- 색상

### [펜과 브러쉬, Old의 의미](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Pen_Brush_Old.md)
- 개요
- 펜(Pen)
- GDI_Blue_Pen 프로젝트
- 브러쉬(Brush)
- Brush 프로젝트
- Old의 으미

### [그리기 모드와 ROP2 모드, 선 그리기](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/RopMode.md)
- 개요
- Win32 API의 그리기 모드
- Win32 API가 지원하는 그리기 모드
- RopMode를 활용해 선 그리기
- 마우스 왼쪽 버튼을 누르면 클릭 상태가 유지된다.
- 드래그하면 그 만큼 선을 그리게 된다.
- 마우스를 떼면 선 그리기가 완료되고 더 이상 선이 그려지지 않는다.

### [비트맵 출력](https://github.com/bluestronica/bluestronica.github.io/blob/main/WindowsAPI/Bitmap.md)
- 개요
- 비트맵(Bitmap)
- 비트맵 출력
- 비트맵 저장
- 리소스 파일 생성
- 코드 작성



