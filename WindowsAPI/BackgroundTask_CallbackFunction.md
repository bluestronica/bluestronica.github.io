[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 타이머를 활용해서 백그라운드 작업을 할 수 있다. 또한, 타이머는 윈도우 프로시저 내에서 WM_TIMER 메세지를 처리하는 방식으로 활용했다. 
- 작은 프로그램에선 상관이 없지만, 프로그램의 크기가 커진다면 윈도우 프로시저 내에서 모든 것을 처리한다면 복잡성이 올라가고 가독성도 떨어질 것이다.  
- 이번 포스팅에서는 타이머를 활용해 백그라운드 작업을 하는 것과 콜백 함수를 통해서 타이머를 처리하는 것에 대해서 알아보도록 하겠다.


# 백그라운드 작업
- Windows API는 백그라운드 작업을 지원한다. 어떤 작업을 처리하는 도중에 다른 메세지가 발생한다면 해당 메세지를 처리 할 수 있어야 한다. 즉, 발생한 메세지 중 필요한 작업을 해야한다는 뜻이다. 
- 이 말은, 한 프로그램이 제어권을 독점하고 있어면 안된다는 것이다.
- 이를 테면, 무한루프가 발생하는 프로그램을 작성하는 것이다. 반복문이 돌고 있는 도중에는 다른 작업을 할 수 없기 때문이다.
- 타이머를 이용해서 특정 작업이 리소스(자원)를 독점하는 것을 막을 수 있다.

### RandGrp 프로젝트
- 배경에 1000개의 랜덤 좌표에 임의의 색상을 출력하는 것이다.
- 이 작업을 초당 20번 주기로 반복한다.
- 이와 동시에, 왼쪽 마우스 버튼을 클릭하면 원을 그리는 것을 작성해본다.
- 배경을 출력하기 위해 메세지를 처리하고 있는 도중에 마우스가 클릭되었다는 메세지가 발생하면 해당 메세지를 처리하는 것이다.

### 반복문에서 제어권을 독점하고 있는 올바르지 못한 코드
- 프로그램이 시작되면 랜덤 좌표에 임의의 색깔의 원이 프로그램이 종료할 때까지 출력되어야 한다. 
- 끝도 없이 계속 진행되어야 하기 때문에 다음과 같이 생각 할 수 있다.
```c
case WM_PAINT
  hdc = BeginPaint(hWnd, &ps);
  while(1)
  {
    SetPixel(hdc, rand()%500, rand()%400, RGB(rand()%256, 
              rand()%256, rand()%256);
  }
  EndPaint(hWnd, &ps);
  return 0;
```
- 하지만, 이렇게 작성되면 다른 메세지가 발생했을 때 발생한 메세지를 처리 할 수 없다.
- 반복문에서 제어권을 독점하고 있기 때문에 올바르지 못한 코드이다.

### 제어권을 독점하지 않고, 타이머를 활용해서 백그라운드 작업을 하는 프로그램의 원도우 프로시저
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	int i;

	switch (iMessage)
	{
	case WM_CREATE:
		SetTimer(hWnd, 1, 50, NULL);
		return 0;
	case WM_TIMER:
		hdc = GetDC(hWnd);
		for (i = 0; i < 1000; i++)
		{
			SetPixel(hdc, rand() % 500, rand() % 400, 
				RGB(rand() % 256, rand() % 256, rand() % 256));
		}
		ReleaseDC(hWnd, hdc);
		return 0;
	case WM_LBUTTONDOWN:
		hdc = GetDC(hWnd);
		Ellipse(hdc, LOWORD(lParam) - 10, HIWORD(lParam) - 10, 
      LOWORD(lParam) + 10, HIWORD(lParam) + 10);
		ReleaseDC(hWnd, hdc);
		return 0;
	case WM_DESTROY:
		KillTimer(hWnd, 1);
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```


































