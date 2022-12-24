[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- GDI Object 중 하나인 펜과 브러쉬를 활용해 도형을 그렸다. 
- 이번에는 그리기 모드와 RopMode에 대해 다뤄보겠다.
- Win32 API를 활용해 만든 프로그램의 모션은 조금 어색하다.
- 최근 컴퓨터의 처리 능력이 높아져서 덜하지만, 버벅이거나 끊기는 듯한 느낌은 어쩔 수 없이 들게된다.
- 이러한 부분을 보완하기 위해서 RopMode를 활용한다. 
- 특히, RopMode를 활용하여 애니메이션 효과를 줄 수도 있다.


# Win32 API의 그리기 모드
- 그리기 모드란, 도형이 그려질 때 원래 그려져 있던 그림과 새로 그려지는 그림과의 관계를 정의하는 것을 의미한다. 
- 이는 원래 도형이 그려져있던 곳에 새로운 도형을 그릴 때 어떻게 처리할지에 대한 것이다.

### Win32 API가 지원하는 그리기 모드

| 그리기 모드 | 설명 |
|:---|:---|
|R2_BLACK|항상 검은색|
|R2_WHITE|항상 흰색|
|R2_NOP|아무런 그리기도 하지 않는다.|
|R2_NOT|원래의 그림을 반전시킨다.|
|**R2_COPYPEN**|**원래의 그림을 덮어버리고 새 그림을 그린다.**|
|R2_NOTCOPYPEN|새 그림을 반전시켜 그린다.|
|R2_MERGEPEN|OR연산으로 두 그림을 합친다.|
|R2_MASKPEN|AND연산으로 겹치는 부분만 그린다.|
|2_XORPEN|XOR연산으로 겹치는 부분만 반전시킨다.|

- 그리기 모드의 디폴트는 R2_COPYPEN으로, 기존에 그려져 있던 그림 위에 새로운 그림을 그리면 덮어씌우게 된다.
- 그리기 모드 변경함수와 설정된 그리기 모드를 사용하기 위해 가져오는 함수를 지원하고 있다.
```c
int SetROP2(HDC hdc, int fnDrawMode);  // 그리기 모드 변경 함수
int GetROP2(HDC hdc);  // 그리기 모드를 가져오는 함수
```
- 그리기 모드 변경 함수인 SetROP2 함수의 두 번째 인자인 fnDrawMode에 위 표에 있는 그리기 모드를 넣어주시면 그리기 모드를 변경할 수 있다.


# RopMode를 활용해 선 그리기
- 직선을 그리는 프로젝트는 3단계로 이루어진다. 
- 마우스 왼쪽 버튼을 누르면 상태가 유지되고,
- 그래그 하면 그 만큼의 선이 그려지고,
- 버튼을 떼면 선 그리기가 완료되고 더 이상 선이 그려지지 않는다.
- 간 단계에 맞게 코드를 나누어 확인해 보자.

### 마우스 왼쪽 버튼을 누르면 클릭 상태가 유지된다.
```c
LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage,
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	static int sx, sy, oldx, oldy;
	static bool bNowDraw = FALSE;

	switch (iMessage) {
	case WM_LBUTTONDOWN:
		sx = LOWORD(lParam);
		sy = HIWORD(lParam);
		oldx = sx;
		oldy = sy;
		bNowDraw = TRUE;
		return 0;
    ...
```
- 마우스를 누르고 있으면 눌린 상태가 유지되어 그림을 그려야 한다. 
- 이를 BOOL 형 구조체 변수 bNowDraw로 이를 확인하게 된다. 
- 그리고 선을 그리기 위해 마우스의 좌표 정보를 저장한다. 
- 좌표 정보를 저장하기 위한 변수가 4개이다. 
- sx, sy는 처음 마우스를 누른 위치 정보를 저장하는 것이고, 
- oldx, oldy는 지워져야 할 선의 끝 좌표를 갖게 된다. 

### 드래그하면 그 만큼 선을 그리게 된다.
```c
case WM_MOUSEMOVE:
	if (bNowDraw) {
		hdc = GetDC(hWnd);
		SetROP2(hdc, R2_NOT);
		MoveToEx(hdc, sx, sy, NULL);
		LineTo(hdc, oldx, oldy);
		oldx = LOWORD(lParam);
		oldy = HIWORD(lParam);
		MoveToEx(hdc, sx, sy, NULL);
		LineTo(hdc, oldx, oldy);
		ReleaseDC(hWnd, hdc);
	}
	return 0;
```
- 마우스가 눌린 상태로 드래그를 하면 그 만큼의 선을 그리게 된다. 조건문을 통해 bNowDraw가 참이게 되면 코드를 실행하게 된다.
- 그림을 그리기 위해 DC를 얻고 ROP 모드를 R2_NOT으로 변경하는 것을 볼 수 있다.
- R2_NOT은 겹치는 부분을 반전시키는 옵션이 있다.
- MoveToEX, LineTo 함수를 통해서 선을 그린다.
- 그리고, 좌표 정보를 최신화해서 한 번 더 그리는 모습을 볼 수 있다.
- 왜 선을 두 번 긋는 것일까?
- 선을 왜 2 번 긋는지 생각해보기 전에, 왜 ROP 모드를 R2_NOT으로 설정했는지 생각해 볼 필요가 있다.
- 검은색 선을 그리고 있다. 여기에 새로운 선을 덧그리면 반전되면서 흰색으로 바뀐다. 
- 이것은 얼핏 보기에 지워지는 효과를 줄 수 있다. 이 효과를 활용하기 위해 선을 2번 그리는 것이다.
- 마우스가 눌린 상태로 마우스를 움직이는 동안 계속해서 글미을 그리고 있다.
- R2_NOT 모드가 아니라면 무수히 많은 선을 볼 수 있다. 하지만, 결국 그려져야 하는 선은 1줄이다.
- 그래서 sx, sy에 처음 마우스를 클릭한 곳을 좌표를 저장하고,
- oldx, oldy에 변경된 마우스의 좌표를 계속해서 최신화해서, 마우스를 뗄 떼까지 가장 최근의 마우스 좌표에 선을 긋는 것이다. 그리고, 이전에 그려져 있는 선을 덧그려 지우는 것이다.
- 쉽게 말하면, 맨 처음 선을 그리고, 덧그려서 선을 지운 뒤, 좌표를 최신화 해서 새로운 위치를 선을 긋는 것이다.

### 마우스를 떼면 선 그리기가 완료되고 더 이상 선이 그려지지 않는다.
```c
	case WM_LBUTTONUP:
		bNowDraw = FALSE;
		hdc = GetDC(hWnd);
		MoveToEx(hdc, sx, sy, NULL);
		LineTo(hdc, oldx, oldy);
		ReleaseDC(hWnd, hdc);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 마우스를 떼면 bNowDraw가 FALSE로 바뀌면서, 더 이상 선이 그려지지 않는다. 
- 마지막으로 선을 그리면서 이전의 선을 지워주고, 결국 한 줄만 남게 된다.


