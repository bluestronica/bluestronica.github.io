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
	
	WndClass.cbClsExtra = 0;
	WndClass.cbWndExtra = 0;
	WndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	WndClass.hCursor = LoadCursor(NULL, IDC_ARROW);
	WndClass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
	WndClass.hInstance = hInstance;
	WndClass.lpfnWndProc = WndProc;
	WndClass.lpszClassName = lpszClass;
	WndClass.lpszMenuName = NULL;
	WndClass.style = CS_HREDRAW | CS_VREDRAW;	
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
```c
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
- **APIENTRY**
  - APIENTRY는 윈도우 표준 호출 규약인 `_stdcall`을 사용한다는 의미이다.
  - `_stdcall` 의 특징은 파라미터의 처리를 호출된 쪽에서 한다는 것이다.
  - 원래 우리가 함수를 사용할 때, 인수를 포함한 함수를 사용할 때 처리된 값을 넣어서 함수를 호출하게 된다.
  - 반면, `_stdcall` 을 사용하는 WinMain은 발생한 메세지를 전달만하고 메세지에 대한 처리는 메세지 처리 젼용 함수인 WndProc에서 하게 되는 것이다.
- **인수**
  - _In_과 _In_opt_ 
    - 이것은 Visual Studio가 2019로 넘어오면서 파라미터에 접두사를 추가해서 작성하는 것을 권장하기 때문에 사용하는 것이다. 
- **HINSTANCE hInstance**
  - 프로그램의 인스턴스 핸들이다.
  - 전역으로 선언한 전역변수 g_hInst는 파라미터로 받은 hInstance(인스턴스 핸들) 값을 넘겨 받는다.
  - 인스턴스의 핸들을 WinMain에서 선언해버리면 지역변수로 WinMain에서만 사용할 수 있게된다.
  - 정리하면, 파라미터로 받은 인스턴스의 핸들 값을 전역 변수에 전달해서 다른 함수들이 사용할 수 있도록 하는 것이다.

- **HINSTANCE hPrevInstance**
  - hPrevInstance는 이전에 실행된 프로그램의 인스턴스 핸들이다.
  - 이전에 실행된 프로그램이 없을 경우 NULL을 갖는다. 하지만 지금은 사용되지 않는 파라미터이다.
  - 호환성을 위해서 존재하는 파라미터이므로 신경쓰지 않아도 된다. 따라서 WIN32에서는 항상 NULL 값이다.
- **LPSTR lpszCmdParam**
  - 명령행(도스,터미널)으로 입력된 프로그램 인수이다. 도스의 argv 인수에 해당된다. 마찬가지로 거의 사용되지 않는다. 
- **int nCmdShow**
  - 프로그램이 실행될 형태이다.
  - 최소화, 보통 상태일 때 모양이 전달된다.
- 변수 선언
  - **`HWND hWnd;`**
    - 윈도우의 핸들을 선언한다. 윈도우 핸들 구조체에 관련 정보가 담겨있다.
  - **`MSG Message;`**
    - 메세지 구조체를 선언한다. 메세지에 대한 정보가 구조체에 담경있다.
  - **`WNDCLASS WndClass;`**
    - 윈도우 클래스를 선언한다. 다음이 이 윈도우 클래스를 정의하는 부분이다.
  - **`g_Hinst = Hinstance;`**
    - 파라미터로 받은 인스턴스의 핸들을 전역 변수에 저장하는 것이다.
  
### 윈도우 클래스의 정의






















