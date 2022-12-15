[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# 긴 문자열 출력하기

### DrawText 프로젝트
```c
LRESULT CALLBACK WndProc(HWND hWnd, 
  UINT iMessage, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	RECT rt = { 50, 20, 350, 220 };
	LPCTSTR str = _T("이전 포스팅에서 문자열을 출력하기 위해 TextOut 함수를 사용했다.")
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
