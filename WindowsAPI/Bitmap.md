[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 그래픽 그리기 위한 스톡 오브젝트를 배웠다.
- 스톡 오브젝트의 대표적인 예로 펜, 브러쉬 등이 있었지만 간단한 선, 도형을 그리는 것은 어렵지 않지만, 높은 퀄리티의 도형을 기대하기는 쉽지 않다.
- 따라서, 미리 그려져 있는 그래픽을 불러오는 방법을 생각해봐야 한다.
- 미리 작성되어 있는 비트맵을 출력하는 것에 대해 알아보자.

# 비트맵(Bitmap)
- 비트맵은 아이콘, 그림, 사진 등과 같이 디지털 이미지를 어떠한 형식을 가지고 저장된 리소스를 뜻한다.
- 어떠한 형식, 즉 특정 포맷을 가지고 저장된 파일을 의미한다. 옛날 게임이나 메탈슬러그 게임 등 주변에서 한 번쯤은 본 적이 있을 것이다.
- 이러한 비트맵을 화면에 출력하는 절차는 다음과 같다.
  - 현재 화면 DC와 호환되는 새로운 DC를 생성한다. (MemDC)
  - MemDC에 비트맵을 Select 한다.
  - BitBlt와 같은 함수를 사용해 DC간 고속 복사를 수행하여 화면에 출력한다.
- 그냥 화면에 띄우면 될 것 같지만, 위와 같이 다소 복잡한 방법으로 비트맵을 출력하고 있다. 비트맵을 메모리에 미리 저장해놓고 빠르게 가져오기 위해서 위와 같은 방법을 사용하고 있다. 
- 만약, 비트맵을 화면에 바로 띄우면 어떻게 될까? 화면에 출력하기 위해서, 먼저 비트맵을 로딩해야 합다. 비트맵의 품질이 높을 수록 큰 용량을 차지할 것이며, 이는 많은 시간을 차지할 것이다. 
- 또한, 화면에 출력하는 등 많은 시간이 걸려 프로그램이 느려질 수 있다. 특히나, 이미지기 때문에 속도의 저하는 사용자에게 크게 체감될 것이다.
- 따라서, 비트맵을 메모리에 미리 등록해놓고, 고속복사 등을 통해 가져옴으로써 빠르게 비트맵을 출력하는 것이다.

# 비트맵 출력
- 위와 같은 방법으로 사용해 비트맵을 화면에 출력한다.

### 비트맵 저장
- 먼저, 프로그램을 작성하기전에 비트맵을 워킹 디렉토리에 저장해야 한다.
- 워킹 디렉토리란, 현재 작업하고 있는 폴더를 의미한다. 
- 여기서는 .cpp 파일이 저장되어 있는 프로젝트 폴더를 의미한다. 
- 프로그램을 컴파일 할 때, 컴파일러가 인식할 수 있는 폴더 내에 비트맵을 저장해야 한다. 
- 따라서, 다음과 같이 .cpp 파일과 동일한 디렉토리에 .bmp(비트맵) 파일을 저장합니다.

### 리소스 파일 생성
- 이후, 생성한 프로젝트에서 리소스 파일을 새로 생성해야 한다. 
- 다음과 같이, "리소스 파일"을 우클릭하여 추가-새 항목 을 클릭한다.
- 그럼, 기존 소스 파일을 확인하던 오른쪽 부분에 리소스 뷰가 다음과 같이 뜬다. 
- 다시 우클릭 후, 리소스 추가를 선택한다.
- 그럼 다음과 같이 로딩된 비트맵이 뜨게 된다. 비트맵을 수정할 수 있다. (IDE(VS2019)에서 지원하는 기능으로 알고 있다.) 
- 비트맵을 보면, 4비트, 8비트 등등 다양한 비트가 지정되어 있다. 
- 이는 비트맵에 나타낼 수 있는 색깔의 조합을 의미한다. 
- 비트 수가 많을 수록 다양한 색깔을 사용하여 비트맵을 표현할 수 있지만, 당연히 비트맵에 필요한 리소스(용량)은 커지게 된다.
- 이후, 헤더파일의 resoure.h 파일에 #define IDB_BITMAP1 101 로 잘 설정이 되었는지 확인해야 한다.
- 해당 비트맵을 매크로 상수로 관리하여 개발에 용이하게 한다.

### 코드 작성
```c
#include <windows.h>
#include <tchar.h>
#include "resource.h"

//중략

LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage,
	WPARAM wParam, LPARAM lParam)
{
	HDC hdc, MemDC;
	PAINTSTRUCT ps;
	HBITMAP MyBitmap, OldBitmap;

	switch (iMessage) {
	case WM_PAINT:
		hdc = BeginPaint(hWnd, &ps);
		MemDC = CreateCompatibleDC(hdc); // 메모리DC 생성
		MyBitmap = LoadBitmap(g_hInst, MAKEINTRESOURCE(IDB_BITMAP1)); //로딩
		OldBitmap = (HBITMAP)SelectObject(MemDC, MyBitmap); //비트맵 선택
		BitBlt(hdc, 0, 0, 123, 160, MemDC, 0, 0, SRCCOPY); //복사 및 출력
		SelectObject(MemDC, OldBitmap);
		DeleteObject(MyBitmap);
		EndPaint(hWnd, &ps);
		return 0;
	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;
	}
	return(DefWindowProc(hWnd, iMessage, wParam, lParam));
}
```
- 이번 프로젝트는 resource.h 파일에 리소스의 매크로 상수를 정의해놨으므로, 해당 파일을 inlucde 해야 한다. 
- 평소에 사용하던 stdio, tchar와 다르게 큰 따옴표로 묶여있는 것을 확인할 수 있다. 큰 따옴표로 묶여있다는 것은 워킹 디렉토리에 존재하는 파일을 가져오겠다는 뜻이다.
- CreateCompatibleDC 함수를 사용해서 기존 메모리와 양립하는, 
- 즉 동일한 특성을 가지며 내부에 출력 표면을 가진 메모리 영역을 생성한다. 
- 해당 메모리 영역에 비트맵을 그린 후, 출력할 화면으로 고속 복사하는 것이다. 
- 이후, SelectObject() 함수를 사용해 리소스 내에 비트맵을 선택한 후, LoadBitmap() 함수를 사용해 비트맵을 읽어오는 것이다.
- BitBlt() 함수는 함수간 영역을 고속 복사하는 함수입니다. 함수의 원형은 다음과 같다.
```c
BOOL BitBlt(1.HDC hdcDest, 2.int XDest, 3.int YDest, 4.int nWidth 5.int nHeight
            6.HDC hdcSrc, 7.int XSrc, 8.int YSrc, 9.DWORD dwRop);
```
  - 1번 파라미터는 복제 대상을 의미합니다. 비트맵을 복제해 화면에 출력할 대상을 의미한다. 
  - 2~5번 파라미터는 복사 대상의 x, y, weith, height를 의미한다. 
  - 6번 파라미터는 복사 원본의 DC, 7, 8번은 복사 원본의 좌표, 9번은 어떻게 복사할지에 대한 정보다.
  - 여기서 사용된 SRCCOPY는 대상 영역에 복사하는 옵션이다. 
  - 이외에도, 값을 반전시키는 DSTINVERT, 검은색으로 채우는 BLACKNESS 등이 있다.
- 당연히, 비트맵 출력 후 비트맵 자체와 메모리 DC를 해제해야 한다.
- 참고로, Win32 API는 비트맵을 바로 출력하는 함수를 지원하지 않는다. 
- 로딩 속도로 인해 실제와 차이가 발생할 수 있기 때문이다.


