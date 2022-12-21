[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# Device Context

### Device Context(DC)란?
- Win32에서 출력을 하기 위해서는 Device Context라는 구조체가 필요하다. 해당 구조체는 출력에 필요한 모든 정보를 갖고 있는 데이터 구조체이다. 이 구조체가 왜 필요한지 이해하기 위해서는 화면에 출력되기 까지의 과정을 알고 있어야 한다.
- 윈도우는 기본적으로 세 가지 동적 연결 라이브러리(Dynamic Linked Library)로 구성 되어 있다.
  - Kernel : 메모리를 관리하고 프로그램을 실행한다.
  - User : 유저 인터페이스(UI)와 윈도우를 관리한다.
  - GDI : Graphic Device Interface의 약자로, 화면처리와 그래픽 등 모든 출력 장치를 제어한다.
    - GDI는 모니터, 프린터와 같은 모든 출력 장치를 제어하는 인터페이스이다. 이렇게 출력 장치를 제어하고, 그 곳에 출력하는 것을 GDI 오브젝트라고 한다. 펜, 브러시, 비트맵, 폰트 등 화면에 출력하는 요소들을 뜻한다. 그리고 이런 GDI 오브젝트를 모아놓은 곳이 Device Context이다. 그렇기 때문에 Device Context가 출력에 필요한 모든 정보를 가지는 데이터 구조체라고 하는 것이다. 
- 정리하자면,
  - GDI가 출력 장치를 제어
  - GDI 모듈이 Device Context를 관리한다.
  - 이 Device Context는 펜, 폰트 등 화면에 출력하는 요소인 GDI Object를 갖고 있다.
  - 이를 활용하여 다양한 요소를 화면에 출력한다.

### Device Context가 필요한 이유?
- 선을 그리기 위해서는 다음과 같은 정보들이 필요하다.
  - 선의 시작점 위치
  - 선의 끝점 위치
  - 선의 형태(실선, 점선 등등)
  - 선의 색깔
  - 선의 굻기
  - 선을 그리는 모드(ROP 모드 등)
- 이처럼 화면에 무언가를 출력하기 위해서는 개발자가 신경써야 할 부분이 많기때문에 Device Context를 이용해서 화면에 출력하는 과정을 거친다.
  - Device Context를 사용한 실제 API 코드는 다음과 같다. 
  - `LineTo(hdc, X, Y);` 선이 그어질 위치만 알려주면, 그 외의 정보들은 Device Context에 저장된 정보를 기반으로 자동으로 처리하는 것을 알 수 있다.
- 그 외에도, 윈도우가 실행되는 환경은 여러 개의 프로그램이 동시에 실행되는 멀티 태스킹 시스템이기 때문에, 다른 프로그램과의 상태에도 영향을 받는다. Device Context를 통해서 운영체제가 인지하여 처리하도록 다른 프로그램의 윈도우끼리 출력 결과가 서로 방해하지 않도록 완충하는 역할도 하고 있다.


# 문자열 출력하기
### TextOut 프로젝트 1
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	switch (iMessage)
	{
	case WM_LBUTTONDOWN:
		hdc = GetDC(hWnd);
		TextOut(hdc, 100, 100, _T("Bluestronica!!"), 15);
		ReleaseDC(hWnd, hdc);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

### 윈도우 프로시저에서 사용할 변수 선언
- 메세지 처리 전용 함수인 WndProc 함수에 위와 같이 메세지 처리 함수를 작성
- HDC는 Handle Device Context의 약자
- DC를 다루는 핸들 구조체인 hdc를 선언
- 그리고 2개의 메세지를 처리

### WM_LBUTTONDOWN 메세지
- WM는 Window Message의 약자이다. 윈도우에서 발생한 메세지인데, 
- L BUTTON DOWN, 왼쪽 버튼 마우스가 눌렸을 때 발생하는 메세지이다. 
- GetDC(hWnd) 함수를 통해 해당 윈도우의 DC를 얻을 수 있다.
- **윈도우의 DC를 얻는 함수는 GetDC, BeginPaint 2개가 있다.**
  - BeginPaint 함수는 WM_PAINT 메세지에서만 사용할 수 있고
  - GetDC 함수는 그 외 메세지에서 사용된다.
- TextOut(DC, X좌표, Y좌표, 문자열, 문자열길이)함수를 통해 해당 텍스트를 출력할 수 있다.
- 문자열 출력을 마치면 ReleaseDC를 통해 DC를 반환해야한다.
- GetDC와 ReleaseDC는 세트라고 생각하면 편하고, 이는 메모리 낭비를 막기 위해 사용된다. 
- 이후, return 0;를 통해 메세지 처리를 종료한다. 
- lParam, wParam의 정보에 따라 메세지 내부에 switch문을 또 사용하는 경우가 있는데, 이 때 break;로 구분하고 메세지 종료는 return 으로 구분한다.

### WM_DESTROY 메세지
- 해당 메세지는 오른쪽 위에 X표시를 누를 경우 발생하는 메세지다. 프로그램이 종료할 때 발생하는 메세지이며 PostQuitMessage(0)을 통해 종료 메세지를 전송한다. 
- 메세지 처리가 완료된 후, 처리 결과를 다시 반환한다.(LRESULT형)


### TextOut 프로젝트 2
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;

	switch (iMessage)
	{
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 100, 100, _T("BluesTronica!!"), 15);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 윈도우를 최소화했다 다시 켜도, 사이즈를 바꿔도 문자열이 사라지지 않는다.

### WM_PAINT 메세지
- 화면에 출력하는 메시지이다.
- WM_PAINT 메세지가 발생하는 시점이 여러 개가 있는데, 그 중에 프로그램이 실행되었을 때도 WM_PAINT 메세지가 발생한다. 
- 프로그램이 시작되면 발생하는 메세지를 처리하고, 윈도우를 띄우기 위해서 WM_PAINT 메세지가 발생하는 것이다. 그래서 윈도우를 시작하자마자 문자열이 출력되는 것이다.

### BeginPaint VS GetDC
- BeginPaint
  - WM_PAINT 메세지에서만 사용하는 함수이다.
  - BeginPaint는 윈도우의 Clipping Region을 자동으로 파악하는 특징이 있다. 
  - Clipping Region이란, 클라이언트 영역의 특정 부분에 그리기를 한정하는 영역을 의미한다. 
  - 위에서 메모장 겹치는 것을 보았듯이, 뒤에 가려져있는 메모장을 클릭하게 되면 가려져 있던 부분을 업데이트해야한다. 
  - 이렇게 윈도우가 생성되거나, 움직이거나, 사이즈가 바뀌거나, 스크롤 되는 등 윈도우의 화면 상의 변화하는 부분을 Clipping Region이라고 한다. 
  - 일부 메세지는 화면의 변화가 아니기 때문에 화면의 변화를 감지하지 못한다. 
  - 예를 들어, 위에서 본 TextOut 프로젝트에서 LBUTTONDOWN 메세지가 발생하면, TextOut함수를 통해서 문자열을 출력해야하는데, 마우스가 클릭된 것은 화면의 변화가 아니기 때문에 WM_PAINT 메세지가 발생하지 않는다. 프로그램을 최소화했다가 다시 띄우면 윈도우를 그리는 WM_PAINT만 발생하고, WM_LBUTTONDOWN 메세지가 발생하지 않기 때문에 문자열이 출력되지 않는 것이다.
  - 그래서 화면의 변화가 있다고 윈도우에 알리기 위해 InvalidateRect 혹은 InvalidateRgn 함수를 사용한다. 이에 대한 자세한 내용은 추후에 다룰 예정이다.
- GetDC
  - WM_PAINT 외에 메세지에서 출력을 하기 위해 해당 Client 영역의 Device Context를 얻는 함수다.
  - TextOut 프로젝트에서 LBUTTONDOWN 메세지가 발생하면 바로 출력하는 것처럼 즉각적인 출력이 이루진다. 하지만, 일시적인 출력 방법으로 그 이후의 변화에는 책임지지 않는다. 
  - 그래서, 윈도우가 최소화되었다가 띄워지거나, 윈도우의 사이즈가 변경되는 경우 문자열을 다시 출력하지 않는 것이다. 
  - 그래서 GetDC 함수는 배경과 관계없이 특정 출력을 일시적(즉각적)으로 반영하고자하는 경우 사용하곤 한다. 여담으로, 윈도우의 타이틀바 영역에 대한 DC를 얻기 위해서는 GetWindowDC() 함수를 사용한다. 
  
### BeginPaint는 정적(Static) 출력, GetDC는 동적(Dynamic) 출력
- BeginPaint 함수는 정적(Static) 출력을 하기 위해 사용한다. 윈도우의 틀이 출력되는 것처럼 기본적으로 출력할 수 있는 것이다. 
- 반대로, GetDC 함수는 동적(Dynamic) 출력을 하기 위해 사용한다. 마우스의 동작에 따라, 혹은 키보드의 입력에 따라 동작할 때 GetDC 함수를 사용하여 바로 출력하곤한다. 
- 일시적인 출력이기 때문에, 다른 윈도우에 의해 가려지거나 윈도우가 변화하면 지워진다.(이후 변화는 책임지지 않기 때문이다.)
- 두 함수 모두, 사용이 끝나면 메모리를 반환하기 위해서 EndPaint(BeginPaint를 사용한 경우),ReleaseDC(GetDC를 사용한 경우)를 꼭 사용해야한다.

### PAINTSTRUCT 구조체
- HDC, rcPaint와 같은 출력에 관한 정보를 담고있는 구조체이다.
- 이 구조체의 주소를 두번째 매개변수로 사용하여 정적 출력을 진행할 수 있다.


# 문자열 정렬
- 출력하는 문자열을 정렬하기 위해 다음과 같은 함수를 사용한다.
- 대부분 요소들에 대한 설정은 함수와 OR연산을 통해서 세팅할 수 있다.
- **` UINT SetTextAlign(HDC hdc, UINT fMode); `**

| 값 | 기능 |
|:---|:---|
|TA_TOP|지정한 좌표가 상단에 위치|
|TA_BOTTOM|지정한 좌표가 하단에 위치|
|TA_CENTER|지정한 좌표가 수평 중앙 좌표|
|TA_LEFT|지정한 좌표가 수평 왼쪽|
|TA_RIGHT|지정한 좌표가 수평 오른쪽|
|TA_UPDATECP|지정한 좌표가 CP(Current Position)를 사용, 출력 후 CP변경|
|TA_NOUPDATECP|CP(Current Position)를 사용하지 않고 좌표사용, CP 변경하지 않음|

### TextOut 프로젝트 3
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;

	switch (iMessage)
	{
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		TextOut(hdc, 100, 60, _T("BluesTronica"), 12);
		TextOut(hdc, 100, 80, _T("is My"), 5);
		TextOut(hdc, 100, 100, _T("Lovely home Country"), 19);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}

// 결과 :  
//      BluesTronica
//      is My
//      Lovely home Country
// 정렬하기 전, 각 좌표에 맞게 해당 문자열이 출력된다. 
// X좌표는 동일하고, Y좌표가 20씩 커지면서 아랫 줄에 출력된다.
```
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, 
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;

	switch (iMessage)
	{
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		SetTextAling(hdc, TA_UPDATECP);
		TextOut(hdc, 100, 60, _T("BluesTronica"), 12);
		TextOut(hdc, 100, 80, _T("is My"), 5);
		TextOut(hdc, 100, 100, _T("Lovely home Country"), 19);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}

// 결과 :  
//      BluesTronica is My Lovely home Country
// TA_UPDATECP 옵션으로 정렬하니 한 줄로 출력되는 모습을 확인할 수 있다.
// 해당 옵션은 출력 위치를 지정하는 인수를 무시하고,
// 항상 CP(Current Position)의 위치에 문자열을 출력하고,
// 출력 후 CP를 문자열의 다음 위치로 옮긴다.
// 그렇기 때문에, 한 줄로 출력되는 것이다.
```

