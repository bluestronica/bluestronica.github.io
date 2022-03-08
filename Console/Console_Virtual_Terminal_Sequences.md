[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# Class Window Console API VS Console Virtual Terminal Sequences
- Windows에서 현재 진행 중인 개발과 모든 신규 개발에서 터미널과 상호 작용하는 방식으로 **가상 터미널 시퀀스**를 사용하는 것이 좋다. 
- 그러면 Windows 명령줄 클라이언트 애플리케이션이 다른 모든 플랫폼에서 애플리케이션 프로그래밍 스타일과 융합된다.
###
- **Class Window Console API**
    - Window에서만 지원된다.
    - 로컬 머신에서만 엑세스할 수 있다.
### 
- **Console Virtual Terminal Sequences**
    - 표준 입력 및 표준 출력 스트림에 포함된 명령 언어로 정의된다. 
    - 가상 터미널 시퀀스는 인쇄할 수 없는 이스케이프 문자를 사용하여 
    - 인쇄 가능한 일반 텍스트로 인터리브된 명령에 신호를 보낸다.
    ###
    - 가상 터미널 시퀀스(Virtual terminal sequences)는 출력 스트림에 쓸 때   
    - 커서 이동, 색/글꼴 모드 및 기타 작업을 제어할 수 있는 제어 문자 시퀀스(control character sequences)이다.  
    - 출력 스트림 쿼리 정보 시퀀스에 대한 응답으로  
    - 입력 스트림에서 시퀀스를 수신하거나 적절한 모드가 설정된 경우  
    - 사용자 입력의 인코딩으로 시퀀스를 수신할 수도 있다.  
    - GetConsoleMode() 및 SetConsoleMode() 함수를 사용하여 이 동작을 구성할 수 있다.  

###
- **Windows Console API 사용의 예외**
    - 프로세스의 표준 핸들은 계속해서 GetStdHandle() 및 SetStdHandle() 을 통해 제어된다.
    - 가상 터미널 시퀀스 지원을 옵트인하는 핸들의 콘솔 모드 구성은 GetConsoleMode 및 SetConsoleMode 를 통해 처리된다.
    - 코드 페이지 또는 UTF-8 지원 선언은 SetConsoleOutputCP 및 SetConsoleCP 메서드를 통해 수행된다.
    - 콘솔 디바이스 세션에 참가하거나 세션에서 나가려면 AllocConsole, AttachConsole 및 FreeConsole을 사용하여 약간의 전체 프로세스 관리를 수행해야 할 수도 있다.
    - 신호 및 신호 처리는 계속해서 SetConsoleCtrlHandler, HandlerRoutine 및 GenerateConsoleCtrlEvent를 통해 수행된다.
    - 콘솔 디바이스 핸들과의 통신은 WriteConsole 및 ReadConsole을 통해 수행할 수 있다. 다음과 같은 형식의 프로그래밍 언어 런타임을 통해 활용할 수도 있다. 
        - C 런타임(CRT): printf, scanf, putc, getc 같은 스트림 I/O 또는 다른 수준의 I/O 함수. 
        - C++ 표준 라이브러리(STL): cout 및 cin 같은 iostream. 
        - .NET 런타임: Console.WriteLine 같은 System.Console.
    - 창 크기 변경을 인식해야 하는 애플리케이션은 계속해서 ReadConsoleInput을 사용하여 키 이벤트로 인터리브된 변경 내용을 수신해야 한다. ReadConsole 단독으로 사용하면 변경 내용이 취소된다.
    - 열 또는 그리드를 그리거나 화면을 채우려고 시도하는 애플리케이션에 대해서는 GetConsoleScreenBufferInfo를 사용하여 창 크기 찾기를 계속 수행해야 한다. 창 및 버퍼 크기는 pseudoconsole 세션에서 매칭된다.

## Output Sequences
- **SetConsoleMode()** 함수를 사용하여 
- 화면 버퍼 핸들(screen buffer handle)에 
- **ENABLE_VIRTUAL_TERMINAL_PROCESSING** 플래그가 설정된 경우
- 콘솔 호스트는 출력 스트림에 기록될 때 터미널 시퀀스를 가로챈다.
- **DISABLE_NEWLINE_AUTO_RETURN** 플래그는 행의 마지막 열에 기록된 문자와 관련하여
- 다른 터미널 에뮬레이터의 커서 위치 지정 및 스크롤 동작을 에뮬레이트하는 데에도 유용할 수 있다.



## Input Sequences
- **SetConsoleMode()** 함수를 사용하여
- 입력 버퍼 핸들(input buffer handle)에
- **ENABLE_VIRTUAL_TERMINAL_INPUT** 플래그가 설정된 경우
- 콘솔 호스트는 입력 스트림의 터미널 시퀀스를 내보낸다.



## Example of Enabling Virtual Terminal Processing
- 애플리케이션에 가상 터미널 처리를 사용하도록 설정하는 방법
    - 기존 모드는 항상 GetConsoleMode()를 통해 검색하여 분석한 후
    - SetConsoleMode()로 설정되어야 한다.
    ###
    - SetConsoleMode()가 ERROR_INVALID_PARAMETER 반환하고 
    - GetLastError()의 반환이 0 되는지 여부를 확인하는 것은 
    - 하위 수준 시스템에서 실행할 때를 확인하는 현재 메커니즘입니다.

```c++
// Set output mode to handle virtual terminal sequences
#include <stdio.h>
#include <wchar.h>
#include <windows.h>

int main()
{
    // 1. 화면 버퍼 핸들을 가져온다.
    HANDLE hOut = GetStdHandle(STD_OUTPUT_HANDLE);
    if (hOut == INVALID_HANDLE_VALUE)
    {
        return false;
    }
    // 2. 입력 버퍼 핸들을 가져온다.
    HANDLE hIn = GetStdHandle(STD_INPUT_HANDLE);
    if (hIn == INVALID_HANDLE_VALUE)
    {
        return false;
    }

    DWORD dwOriginalOutMode = 0;
    DWORD dwOriginalInMode = 0;
    // 화면 버퍼에 대한 콘솔모드를 가져온다..
    if (!GetConsoleMode(hOut, &dwOriginalOutMode))
    {
        return false;
    }
    // 입력 버퍼에 대한 콘솔모드를 가져온다.
    if (!GetConsoleMode(hIn, &dwOriginalInMode))
    {
        return false;
    }
    
    DWORD dwRequestedOutModes = ENABLE_VIRTUAL_TERMINAL_PROCESSING | DISABLE_NEWLINE_AUTO_RETURN;
    DWORD dwRequestedInModes = ENABLE_VIRTUAL_TERMINAL_INPUT;

    // 가져온 화면 버퍼에 대한 콘솔 모드에 가상 터미널 처리 사용을 위한 세팅
    DWORD dwOutMode = dwOriginalOutMode | dwRequestedOutModes;
    // 콘솔 모드 적용
    if (!SetConsoleMode(hOut, dwOutMode))
    {
        // we failed to set both modes, try to step down mode gracefully.
        dwRequestedOutModes = ENABLE_VIRTUAL_TERMINAL_PROCESSING;
        dwOutMode = dwOriginalOutMode | dwRequestedOutModes;
        if (!SetConsoleMode(hOut, dwOutMode))
        {
            // Failed to set any VT mode, can't do anything here.
            return -1;
        }
    }

    // 가져온 입력 버퍼에 대한 콘솔 모드에 가상 터니멀 처리 사용을 위한 세팅
    DWORD dwInMode = dwOriginalInMode | ENABLE_VIRTUAL_TERMINAL_INPUT;
    // 콘솔 모드 적용
    if (!SetConsoleMode(hIn, dwInMode))
    {
        // Failed to set VT input mode, can't do anything here.
        return -1;
    }

    return 0;
}
```


