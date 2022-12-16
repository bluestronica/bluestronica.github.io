[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# 긴 문자열 출력하기

### DrawText 프로젝트
- 사각형 안에 문자열을 그리는 DrawText 함수를 활용해서 긴 문자열 출력
```c
LRESULT CALLBACK WndProc(HWND hWnd, 
  	UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	RECT rt = { 50, 20, 350, 220 };
	LPCTSTR str = _T("문자열을 출력하기 위해 TextOut 함수를 사용했다.")
		_T("말 그대로 문자열을 출력하는 함수다.")
		_T("이번에는 사각형 안에 문자열을 그리는 DrawText 함수를 활용해서")
		_T("긴 문자열을 출력하도록 하겠다.");

	switch (iMessage)
	{
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		DrawText(hdc, str, _tcslen(str), &rt, DT_CENTER | DT_WORDBREAK);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```

### 윈도우 프로시저에서 사용할 변수 선언
- 출력을 위해 HDC 구조체와 PAINTSTRUCT를 선언한다.
- DrawText함수는 사각형 안에 문자열을 출력하기 때문에, 사각형 정보를 정의할 필요가 있다.
- 따라서, RECT구조체를 선언하고 사각형 정보를 정의한다.
```c
typedef struct _RECT
{
	LONG left;
	LONG top;
	LONG right;
	LONG bottom;
} RECT;
```
- 차례대로 { 왼쪽 위, 오른쪽 위, 오른쪽 아래, 왼쪽 아래 }이다.
- 사각형의 왼쪽 위 꼭지점에서 시계방으로 돈다고 생각하면된다.
- RECT 구조체는 상당히 많이 사용하기 때문에 유심히 볼 필요가 있다.
- 이후, 문자열 정의하는데 각 문장 사이에 콤마가 없다.
- 이렇게 긴 문자열을 선언할 때, 콤마를 찍지 않고 마지막 문자열 끝에 세미콜론을 적어주면 된다.

### WM_PAINT 메세지
- DC를 얻기 위해 BeginPaint함수를 사용했고, DrawText함수를 통해 긴 문자열을 출력한다.
- **`DrawText(HDC hDC, LPCTSTR lpString, int nCount, LPRECT lpRect, UINT uFormat);`**
  - HDC hDC : Device Context의 핸들
  - LPCTSTR lpString : 출력될 문자 포인터
  - int nCount : 출력될 문자열의 길이 (-1이면 NULL 종류 문자열로 간주)
  	- _tcslen(문자열) 함수를 통해 문자열의 길이를 구했다.	
  - LPRECT lpRect : 출력될 사각영역 RECT 구조체 포인터
  	- RECT 구조체의 주소를 넣어 사각형 영역 정보를 알려준다.
  - UINT uFormat : 옵션(사각형 안에서 문자열을 정렬하는 것에 대한 정보)
  	- Win32 API에서 옵션이 여려 개가 있을 경우 `|`(OR) 연산을 통해 동시에 사용할 수 있다.
- DrawText 옵션

| 값 | 기능 |
|:---|:---|
|DT_LEFT|수평 왼쪽 정렬|
|DT_RIGHT|수평 오른쪽정렬|
|DT_CENTER|수평 중앙 정렬|
|DT_BOTTOM|사각 영역의 바닥에 문자열 출력|
|DT_VCENTER|사각 영역의 수직 중앙에 문자열 출력|
|DT_WORDBREAK|사각 영역의 오른쪽 끝에서 자동으로 개행|
|DT_SINGLELINE|한 줄로 출력|
|DT_NOCLIP|사각 영역의 겨예를 벗어나도 문자열을 자르지 않고 출력|


# 다양한 그래픽(도형) 출력하기

### Win32 API는 픽셀 하나부터, 선, 원, 사각형 등 다양한 도형을 그릴 수 있는 함수를 지원한다.
- 픽셀
  - **`COLORREF SetPixel(hdc, nXPos, nYPos, corref)`**
  - X, Y 좌표, 색깔 정보
- 선의 시작점
  - **`DWORD MoveToEx(hdc, x, y, lpPoint)`**
  - 선의 시작점의 X, Y 좌표, 이전 Ex 좌표
- 선의 끝
  - **`BOOL LineTo(hdc, xEnd, yEnd)`**
  - 선 끝 점의 X, Y 좌표
- 사각형
  - **`BOOL Rectangle(hdc, nLeftRect, nTopRect, nRightRect, nBottomRect)`**
  - 왼쪽 위부터 시계방향
- 원
  - **`BOOL Ellipse(hdc, nLeftRect, nTopRect, nRightRect, nBottomRect)`**
  - 사각형 내부에 원을 그림
  


















