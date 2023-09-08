### SBCS(Single Byte Character Set) 기반 프로그래밍
- 1바이트로 문자를 표현하는 문자 집합이다.
- ASCII
  - 7bit로 영문자, 숫자, 특수문자, 기호 등 128개 문자를 표현할 수 있다.
  - 1bit는 Parity Bit로 통신 에러 검출을 위해 사용된다.  
- ANSI
  - 8bit로 구성되어 있으며 256개의 문자를 표현할 수 있다. (ASCII 확장판) 

### DBCS(Double Byte Character Set)
- 하나의 문자를 표현하는 code가 1바이트나 2바이트인 encoding을 의미한다.
- windows 환경에서는 SBCS와 DBCS의 특수한 경우로 처리돤다.

  
### MBCS(Multi Byte Character Set) 기반의 프로그래밍
- 한글은 2바이트, 영어는 1바이트 사용하는 형태
  - "ABCD한글"은 총 6글자 이지만 한글 한 글자가 2바이트를 사용하여 표현하기 때문에 문자열의 길이는 8이 된다. 
- SBCS와 DBCS가 호환이 되지 않았을 때
- 두 개다 묶어서 쓰는 형태로 나온 것이 MBCS이다.
- Microsoft의 독자적인 character set 이라고 봐도 무방하다.
- Microsoft Windows의 경우 유니코드를 지원하지 않았던 Windows 9x와의 호환성을 위해 여전히 MBCS를 기본 인코딩으로 사용한다.
- UTF-8이나 UTF-16에 BOM(바이트 순서 표식) 문자가 없으면 무조건 MBCS로 읽어들이는 프로그램들이 존재하기 때문에 Windows용 프로그램들은 UTF-8로 저장 시 BOM을 붙이는 경우가 많은데, 이는 BOM이 없는 UTF-8을 사용하는 타 운영체제에서 인코딩 오류를 일으키게 된다.
- UTF-8을 기본 인코딩으로 도입한 macOS나 2000년대 이후의 리눅스 등은 기본적으로 텍스트 파일을 단일 인코딩으로 읽어들이기 때문에 MBCS 형태로 작성된 텍스트 파일은 별도의 처리를 하지 않는 한 전부 �로 보인다.
- Windows10 19H1 빌드부터 메모장에서 BOM없는 UTF-8을 기본 인코딩으로 사용하도록 바뀌었다.
- 이전까지는 UTF-8 저장 시 무조건 BOM을 붙였다.
- 위와 같은 문제점으로 인해 최근에는 유니코드를 사용하여 다국어를 처리하는 것이 일반적이다.


### WBCS(Wide Byte Character Set) 기반의 프로그래밍
- 한 문자를 표현하기 위해 2바이트를 사용하는 문자 집합이다.
- MBCS와 달리, 모든 문자가 같은 길이의 바이트를 사용하기 때문에
- 문자열 처리에서 발생할 수 있는 문제점을 해결할 수 있다.
- WBCS는 유니코드(Unicode)를 사용하여, 다양한 언어와 문자를 지원할 수 있다.
- WBCS를 사용하면 한 문자당 사용하는 바이트 수가 고정되어 있기 때문에
- 문자열 처리가 더 간단해진다.
- 유니코드
  -  전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰수 있도록 설계된 표준이다.
  -  숫자와 글자, 키와 값이 1:1로 매핑된 형태의 코드로
  -  전 세계의 모든 문자를 특정 숫자(키)와 1:1로 매핑한 것이다.
  -  UTF-8, UTF-16은 유니코드를 사용하는 인코딩 방식이다.
  -  인코딩 방식은 유니코드표의 숫자(키)를 어떻게 표현하느냐에 따른 방식이다.
    -  UTF-8은 'A'를 0x41로 표현한다.
    -  UTF-16은 'A'를 0x0041로 표현한다.


### SBCS, MBCS, WBCS 지원하는 함수들
- Microsoft에서 기존의 아스키 코드를 유지하면서, 유니코드 프로그래밍을 지원하기 위해,
- MBCS, WBCS의 자료형을 구분해서 사용하도록 Visual Studio를 설계하였다.
- 따라서, 유니코드 기반 프로그래밍을 Visual Studio에서 하려면,
- 유니코드형에 맞는 자료형을 써야한다.

#### 문자 타입
|SBCS|WBCS|MBCS + WBCS|
|:---|:---|:---|
|char|wchar_t|TCHAR|

- THCAR은 마이크로 소프트에서 만든 Win32 문자열이다.
- ANSI 및 DBCS 플랫폼의 경우 char형으로
- 유니코드 플랫폼의 경우 TCHAR은 WCHAR 유형과 동의어로 정의됩니다.

 
#### 안전한 문자열 함수
|SBCS|WBCS|MBCS + WBCS|
|:---|:---|:---|
|strlen|wcslen|_tcslen|
|strcpy_s(char)|wcsncpy_s(wchar_t)|_tcsncpy_s(TCHAR)|
|strncpy|wcsncpy|_tcsncpy|
|strcat|wcscat|_tcscat|
|ctrncat|wcsncat||
|strcmp|wcscmp|_tcscmp|
|strncmp|wcsncmp|_tcsncmp|


#### 안전한 입출력 함수
|SBCS|WBCS|MBCS + WBCS|
|:---|:---|:---|
|_snprintf_s(char)|_swprintf_s(wchar_t)|_sntprintf_s(TCHAR)|
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

|-|Win32에서 기본적으로 쓰이는 문자타입|Win32식 문자타입|
|:---|:---|:---|
|SBCS|typedef char|CHAR|
|WBCS|typedef wchar_t|WCHAR|
|-|**NULL 문자로 끝나는 c-style 문자열로 구성될 경우**|**Win32식 문자타입**|
|SBCS|typedef CHAR*|LPSTR|
|WBCS|typedef WCHAR*|LPWSTR|
|-|**CONST 상수 속성을 가질 경우**|**Win32식 문자타입**|
|SBCS|typedef CONST CHAR*|LPCSTR|
|WBCS|typedef CONST WCHAR*|LPCWSTR|

- c-style 문자열은 char 배열에 null값이 들어간 형태를 말한다.
- win32에서는 String처리를 위해서 char* 타입을 그대로 쓰기보다는
- LPSTR 등의 표현으로 대치해 사용 함으로써 개발의 편의성을 돕고있다.
   
```C++
const wchar_t* lpcwstr =  L"LPCWSTR에 문자열 만들기";
size_t lpcwstr_length = wcslen(lpwstr); // lstrlenW()


const char* lpcstr =  "LPCSTR에 문자열 만들기";
size_t lpcstr_length = strlen(lpwstr);


LPCTSTR lpcstr =  _TEXT("LPCSTR에 문자열 만들기");
size_t lpcstr_length = _tcslen(lpwstr);


LPCTSTR lpcstr =  _T("LPCSTR에 문자열 만들기");
size_t lpcstr_length = _tcslen(lpwstr);


---------------------------------------------------
_tcslen, strlen, wcslen -----> ansi C 함수
lstrlenW, lstrlenA ----> Windows api 함수

```
































