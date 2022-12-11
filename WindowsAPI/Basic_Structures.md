[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# Windows API의 기본 구조 1

### WinMain, WndProc 함수로 구성
```c
#include <Windows.h>
#include <TCHAR.h>

LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);
HINSTANCE g_hInst;
HWND hWndMain;
LPCTSTR lpszClass = _T("Turtle Rabbit");
```

### 헤더(header) : `<windows.h>` 와 `<TCHAR.h>`
- `<windows.h>`는 Windows API를 사용하기 위해 포함해야 한다.
- `<TCHAR.h>`는 문자열과 관련 있는 헤더
  - 앞으로 _T형 문자열을 사용한다. 2btye로 표현되는 언어들의 호완성을 위해 유니코드(Unicode)를 사용
  
### WndProc(윈도우 프로시저) 함수
- ` LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM); `
- Windows API로 작성된 프로그램에서 가장 중요한 역할을 하는 WndPorc() 함수이다.
- WndProc는 메세지(이벤트)가 발생하면 이 메시를 전달받아 처리하는 역할을 한다.
- wndProc가 메시지를 처리하고 반환할 때는 (반환형) LRESULT 타입으로 반환한다.
- LRESULT
  - 'L' 접두사는 long타입을 의미
  - `typedef LONG_PTR LRESULT;`
  - LRESULT는 long타입의 새로운 이름일 뿐이다. 메세지 처리 결과를 알려준다는 것을 조금 더 직관적으로 나타내기 위해서 이런 변수명을 만들었다.
  - LRESULT 타입은 Win32 환경에서 메세지 처리를 마친 후 운영체제에 신호를 주기 위한 값(long type)이다. 즉, 윈도우 프로시저가 메세지 처리를 끝냈다고 운영체제에게 알려주는 값이다.
- CALLBACK 함수
  - LRESULT란 반환형 다음에 CALLBACK이라고 적혀있다.
  - CALLBACK 함수는 사용자가 호출하는 함수가 아닌, 특정 트리거(이벤트)에 의해 운영체제가 자동으로 실행하는 함수이다.
  - WndProc(윈도우 프로시저)는 메세지(운영체제가 보내 온)가 발생하면 자동으로 실행되어 메세지를 확인해서 그에 맞는 행동을 취한다.

### 전역 변수 선언
- g_hInst, hWndMain, lpszClass를 전역 변수로 선언한 것을 볼 수 있다.
- ` HINSTANCE g_hInst; `
  - 접두사로 'H'는 Handle(핸들)을 의미한다. 'g_'는 global, 전역변수
  - 즉, g_hInst는 인스턴스의 핸들이다.
  - 인스턴스는 프로그램에 대한 정보를 담고 있는데 매번 매개변수로 전달하는 것은 비효율적이기 때눔에 전역 변수로 생성해서 관리 및 사용한다.
- ` HWND hWndMain; `
  - WND는 Window의 약자이고 hWndMain은 메인 윈도우의 핸들을 나타낸다.
- ` LPCTSTR lpszClass = _T("Knowlege Pool"); `
  - LPCTSTR 타입을 뜯어 보면 
    - LP / C / TSTR 나눠 볼 수 있다.
    - LP는 long Pointer
    - C는 const
    - TSTR은 T형STR, 즉 T형 문자열
    - T문자열의 long형 포인터를 상수화(const)해서 갖고 있겠다는 으미이다.
  - lpszClass
    - lpsz도 포인터로 문자열을 가리킨다는 의미
    - 윈도우를 생성하는 클래스를 관리하는 변수 
      - 타이틀바에 출력되는 문자열




























