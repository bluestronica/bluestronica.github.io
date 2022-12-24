[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 펜과 브러쉬를 활용해 화면에 도형을 출력할 것이다. 
- 기본적으로 GDI Object를 만들어 사용하는 원리는 다음과 같다.
```c
HPEN MyPen, OldPen;         // 1.핸들을 선언한다.

MyPen = CreatePen(~);       // 2.GDI 오브젝트를 생성한다.

OldPen = SelectObject(~);   // 3.OldPen에 이전에 사용하던 핸들을 저장한다.

Rectangle, Ellipse ~        // 4.GDI 오브젝트를 사용한다.

SelectObject(hdc, OldPen);  // 5.선택을 해제한다.

DelectObject(MyPen);        // 6.메모리에서 삭제한다.
```
- 위와 같은 과정을 거쳐 Rectangle을 이용해 사각형을 그리게 된다면 펜을 생성할 때 설정한 것이 적용되어 사각형이 그려진다.
- 펜의 역할은 테두리를 그리는 것이므로 사각형의 테두리가 변경된다. 
- 펜을 활용해 사각형을 그리는 과정을 보면, 핸들을 선언하고, GDI Object를 생성한 다음, OldPen에 이전에 사용하던 오브젝트를 저장한다. 
- 이후, 사용 후 선택을 해제하고 메모리에서 삭제하는 것을 볼 수 있다.

# 펜(Pen)
- Windows API에서 제공하는 스톡 펜(Stock Pen)은 흰색, 검은색, 투명색이다.
- 다른 색상의 펜은 개발자가 직접 만들어서 사용해야 한다. 
- 새로운 펜을 만들기 위한 함수는 CreatePen 함수이다.
- **`HPEN CreatePen(int fnPenStyle, int nWidth, COLORREF crColor);`**
  - 만들어지는 펜의 핸들을 반환하며, 
  - 인수로는 펜으로 그려질 선의 종류, 굵기, 색상이 들어간다.
  - 색상은 RGB(R,G,B)를 이용해 넣어주면 된다.
  - 선의 종류는 PS_SOLID(실선), PS_DOT(점선) 등이 있다.

### GDI_Blue_Pen 프로젝트
- 펜을 생성해 파란색 테두리를 가진 사각형 출력
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	HPEN MyPen, OldPen;

	switch (iMessage)
	{
	case WM_CREATE:
		hWndMain = hWnd;
		return 0;
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		MyPen = CreatePen(PS_SOLID, 5, RGB(0, 0, 255));
		OldPen = (HPEN)SelectObject(hdc, MyPen);
		Rectangle(hdc, 30, 30, 200, 100);
		SelectObject(hdc, OldPen);
		DeleteObject(MyPen);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 함수 내부에 MyPen, OldPen이라는 핸들(HPEN)을 선언했다.
- WM_PAINT 메세지 처리 부분에서 각 핸들에 값을 세팅하여 Rectangle 함수를 통해 사각형을 출력했다.
- 사용이 끝난 GDI Object는 메모리에서 해지하는 모습을 볼 수 있다.
- 사용자가 생성한 GDI Object는 메모리를 사용하고 있기 때문에 사용 후 반드시 DeleteObject 함수를 이용해 해제 해야한다.
- 단, **현재 사용하고 있는 오브젝트는 삭제할 수 없다. 그렇기 때문에 OldPen을 생성해 값을 저장하는 것이다.**
- SelectObjec 함수는 대부분 GDI 오브젝트에서 사용하므로, 각 오브젝트에 맞게 캐스팅(형변환)하여 사용해야 한다.


# 브러쉬(Brush)
- 펜과 마찬가지로 브러쉬를 통해 사각형의 내부를 설정할 수 있다. 
- 펜과 다르게 브러쉬는 2가지 종류가 존재한다.
- 내부를 색깔로 채우는 브러쉬와 무늬를 지정하는 브러쉬가 존재한다.
- 각각의 함수는 다음과 같다.
```c
HBRUSH CreateSolidBrush(COLORREF crColor); // 단색 브러쉬
HBRUSH CreateHatchBrush(int fnStyle, COLORREF clrref); // 색상과 무늬를 지정하는 브러쉬
```
- 브러쉬에 적용할 수 있는 무늬는 여러 가지가 존재한다. 
- 수평(HS_HORIZONTAL), 수직선(HS_VERTICAL)으로 채울 수 있는 브러쉬부터 바둑판 모양(HS_CROSS), 좌하향 줄무늬(HS_BDIAGONAL) 등이 있다.

### Bursh 프로젝트
- 파란색 사각형의 내부를 초록색 우하향 줄무늬 모양으로 채우기
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	HBRUSH MyBrush, OldBrush;
	HPEN MyPen, OldPen;

	switch (iMessage) {
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		MyBrush = CreateHatchBrush(HS_BDIAGONAL, RGB(0, 255, 0));
		OldBrush = (HBRUSH)SelectObject(hdc, MyBrush);
		MyPen = CreatePen(PS_SOLID, 5, RGB(0, 0, 255));
		OldPen = (HPEN)SelectObject(hdc, MyPen);
		Rectangle(hdc, 30, 30, 200, 100);
		SelectObject(hdc, OldBrush);
		SelectObject(hdc, OldPen);
		DeleteObject(MyBrush);
		DeleteObject(MyPen);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 펜과 마찬가지로 브러쉬도 핸들을 선언하여 사용하고 있다. 
- 사용한 GDI 오브젝트는 모두 메모리를 해지하고 있는 것을 볼 수 있다.

### Old의 의미
- 펜과 브러쉬 둘 다 My~, Old~로 2개씩 선언하여 사용하고 있다. 
- 사용할 GDI 오브젝트 한 개만 사용한다면 문제가 발생한다.
- 한 개만 사용한다고 가정하고 다음 코드를 확인해보겠다.
```c
case WM_LBUTTONDOWN:
    hdc = GetDC(hWnd);
    for(i=0; i<1000; i++) {
        hPen = CreatePen(PS_SOLID, 2, RGB(255,0,0));
        SelectObject(hdc, hPen);
        Rectangle();
    }
    ReleaseDC(hWnd, hdc);
    return 0;
}
```
- 반복문이 돌면서 Pen을 계속해서 생성한다. 
- 생성하는 그 순간에도 메모리는 계속해서 사용되고 있다. 
- 이것은, 메모리를 해지하지 않았기 때문에 발생하는 문제이다. 
- 반복문 내부에서 생성과 삭제를 계속하면 된다고 생각할 수 있지만, 
- 사용 중인 GDI 오브젝트는 DeleteObject 함수를 사용해 메모리를 해지할 수 없다. 
- 가령, 삭제한다고 하더라도, 디폴트가 삭제되는 문제가 발생한다. 
- 그렇기 때문에, DC에 선택된 GDI 오브젝트는 반드시 하나는 존재해야 한다. 
- 그렇기 때문에 Old가 존재하는 것이다. 


