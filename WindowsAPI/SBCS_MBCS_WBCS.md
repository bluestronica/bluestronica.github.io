### SBCS 기반 프로그래밍
- 1바이트 사용
- 아스키코드도 SBCS 문자 형태


### MBCS 기반의 프로그래밍
- 한글은 2바이트, 영어는 1바이트 사용하는 형태
- 마이크로소프트의 독자적인 character set 이라고 봐도 무방하다.


### WBCS 기반의 프로그래밍
- 모든 문자를 2바이트로 사용
- 대표적으로 유니코드가 WBCS 문자 형태
- 유니코드 기반으로 프로그래밍 하는 것이 안정적이고 좋다.
- 유니코드로 프로그래밍을 시작했으면 끝까지 유니코드로 하는 것이 좋다.


### SBCS, MBCS, WBCS 지원하는 함수들
- Microsoft에서 기존의 아스키 코드를 유지하면서, 유니코드 프로그래밍을 지원하기 위해,
- MBCS, WBCS의 자료형을 구분해서 사용하도록 Visual Studio를 설계하였다.
- 따라서, 유니코드 기반 프로그래밍을 Visual Studio에서 하려면,
- 유니코드형에 맞는 자료형을 써야한다.

#### 문자열 함수
|SBCS|WBCS|MBCS + WBCS(only MS)|
|:---|:---|:---|
|strlen|wcslen|_tcslen|
|strcpy|wcscpy|_tcscpy|
|strncpy|wcsncpy|_tcsncpy|
|strcat|wcscat|_tcscat|
|ctrncat|wcsncat||
|strcmp|wcscmp|_tcscmp|
|strncmp|wcsncmp|_tcsncmp|

#### 입출력 함수
|SBCS|WBCS|MBCS + WBCS(only MS)|
|:---|:---|:---|
|printf|wprintf|_tprintf|
|scanf|wscanf|_tscanf|
|fgets|fgetws|_fgetts|
|fputs|fputws|_fputts|

#### "l"이 있는 함수들
- Win32API에 있는 함수들은 ANSI버전(기본적으로 MBCS), UNICODE버전(WBCS)을 둘 다 지원한다.
- 부연설명 하자면,
- "l" 이 있는 함수 lstrlen은 lstrlenA( ANSI ), lstrlenW( UNICODE )를
- 포함하는 매크로된 함수이다.
- "l" 을 붙이고 끝에 A면 MBCS 방식이고 W면 WBCS 방식이 된다.

### Win32 에서 쓰이는 문자형

|Win32에서 기본적으로 쓰이는 문자형|Win32식 문자형|
|:---|:---|
|typedef char|CHAR|
|typedef wchar_t|WCHAR|
|**NULL 문자로 끝나는 c-style 문자열로 구성될 경우**||
|typedef CHAR*|LPSTR|
|typedef WCHAR*|LPWSTR|
|**CONST 상수 속성을 가질 경우**||
|typedef CONST CHAR*|LPCSTR|
|typedef CONST WCHAR*|LPCWSTR|

```C++
const wchar_t* lpcwstr =  L"LPCWSTR에 문자열 만들기";
size_t lpcwstr_length = wcslen(lpwstr); // lstrlenW()


_tcslen, strlen, wcslen -----> ansi C 함수
lstrlenW, lstrlenA ----> Windows api 함수
-----------------------------------------------------------


const char_t* lpcstr =  "LPCSTR에 문자열 만들기";
size_t lpcstr_length = strlen(lpwstr);


LPCTSTR lpcstr =  _TEXT("LPCSTR에 문자열 만들기");
size_t lpcstr_length = _tcslen(lpwstr);


LPCTSTR lpcstr =  _T("LPCSTR에 문자열 만들기");
size_t lpcstr_length = _tcslen(lpwstr);
```
































