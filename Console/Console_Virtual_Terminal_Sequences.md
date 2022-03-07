[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# Console Virtual Terminal Sequences
###
- 가상 터미널 시퀀스(Virtual terminal sequences)는 출력 스트림에 쓸 때   
- 커서 이동, 색/글꼴 모드 및 기타 작업을 제어할 수 있는 제어 문자 시퀀스(control character sequences)이다.  
###
- 출력 스트림 쿼리 정보 시퀀스에 대한 응답으로  
- 입력 스트림에서 시퀀스를 수신하거나 적절한 모드가 설정된 경우  
- 사용자 입력의 인코딩으로 시퀀스를 수신할 수도 있다.  
###
- GetConsoleMode() 및 SetConsoleMode() 함수를 사용하여 이 동작을 구성할 수 있다.  


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
#include <stdio.h>
#include <wchar.h>
#include <windows.h>

int main()
{
    // Set output mode to handle virtual terminal sequences
    HANDLE hOut = GetStdHandle(STD_OUTPUT_HANDLE);
    if (hOut == INVALID_HANDLE_VALUE)
    {
        return false;
    }
    HANDLE hIn = GetStdHandle(STD_INPUT_HANDLE);
    if (hIn == INVALID_HANDLE_VALUE)
    {
        return false;
    }

    DWORD dwOriginalOutMode = 0;
    DWORD dwOriginalInMode = 0;
    if (!GetConsoleMode(hOut, &dwOriginalOutMode))
    {
        return false;
    }
    if (!GetConsoleMode(hIn, &dwOriginalInMode))
    {
        return false;
    }

    DWORD dwRequestedOutModes = ENABLE_VIRTUAL_TERMINAL_PROCESSING | DISABLE_NEWLINE_AUTO_RETURN;
    DWORD dwRequestedInModes = ENABLE_VIRTUAL_TERMINAL_INPUT;

    DWORD dwOutMode = dwOriginalOutMode | dwRequestedOutModes;
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

    DWORD dwInMode = dwOriginalInMode | ENABLE_VIRTUAL_TERMINAL_INPUT;
    if (!SetConsoleMode(hIn, dwInMode))
    {
        // Failed to set VT input mode, can't do anything here.
        return -1;
    }

    return 0;
}
```


