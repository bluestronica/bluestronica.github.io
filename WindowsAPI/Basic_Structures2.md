[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# Windows API의 기본 구조 2

```c
#include <Windows.h>
#include <TCHAR.h>

LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);
HINSTANCE g_hInst;
HWND hWndMain;
LPCTSTR lpszClass = _T("Turtle Rabbit");

int APIENTRY WinMain(_In_ HINSTANCE hInstance, 
                     _In_opt_ HINSTANCE hPrevInstance, 
                     _In_ LPSTR lpszCmdParam, 
                     _In_ int nCmdShow)
{
	HWND hWnd;
	MSG Message;
	WNDCLASS WndClass;
	g_hInst = hInstance;
}
```

### WinMain 함수의 역할
- WinMain 함수는 다음과 같이 5개의 기능을 한다.
  - **`WndClass 정의 >> WndClass 등록 >> 윈도우 생성 >> 윈도우 출력 >> 메세지 루프`**
- **WndClass 정의**
  - WndClass는 윈도우 클래스를 의미한다. 윈도우 기반이 되는 클래스를 윈도우 클래스라고 한다.
  - WndClass 정의는 만들고자 하는 윈도우의 속성을 정의하는 것을 의미한다.
  - 윈도우의 배경색, 아이콘, 스타일 등등 속성을 정의 할 수 있다.
  - 그뿐아니라, 윈도우에 발생하는 메세지는 어떤 함수에서 처리할지, 해당 인스턴스(프로그램)의 핸들은 무엇인지와 같은 정보들을 정의할 수 있다.
- **WndClass 등록**
  - 윈도우의 속성을 정의하면 윈도우 클래스를 등록해야 한다.
  - 정의된 윈도우를 운영체제가 윈도우 클래스를 인지 할 수 있도록 커널에 등록하는 과정을 거쳐야한다.
- **윈도우 생성**
  - 윈도우 클래스가 커널에 등록되면 메모리에 윈도우 클래스를 올릴 수 있다.
  - 메모리에 윈도우를 만드는 것을 "윈도우 생성"이라고 한다.
  - 메모리에 윈도우가 생성되면 이에 대한 핸들값을 알 수 있다.
- **윈도우 출력**
  - 윈도우가 커널에 등록되고 메모리에 생성되면 이제 사용자에게 보여줘야 한다.
  - 시각화(화면에 출력)하는 것도 WinMain 함수의 역할이다.
- **메시지 루프**
  - 화면에 출력되고 사용자가 사용하기 시작하면 여러가지 메세지(이벤트)가 발생한다.
  - 메세지가 언제 들어올지 알 수 없기 때문에 컴퓨터는 메세지 루프를 돌면서 메세지가 발생하는 것을 기다리고 있다.
  - WinMain에 메세지가 발생할 때까지 메세지 루프를 돌다가, 메세지가 발생하면 해당 메세지를 메세지 처리 전용 함수(윈도우 프로시저, WndProc)로 메세지를 전달한다. 메세지 큐에 쌓여 있는 메세지를 꺼내와 WndProc에 전달하게 된다.
  
### WinMain의 파라미터와 변수 선언
- **APIENTRY**
  - APIENTRY는 윈도우 표준 호출 규약인 `_stdcall`을 사용한다는 의미이다.
  - `_stdcall` 의 특징은 파라미터의 처리를 호출된 쪽에서 한다는 것이다.
  - 원래 우리가 함수를 사용할 때, 인수를 포함한 함수를 사용할 때 처리된 값을 넣어서 함수를 호출하게 된다.
  - 반면, `_stdcall` 을 사용하는 WinMain은 발생한 메세지를 전달만하고 메세지에 대한 처리는 메세지 처리 젼용 함수인 WndProc에서 하게 되는 것이다.
- **인수**


























