[bluestronica.github.io/C](https://bluestronica.github.io/WindowsAPI)


# Windows Data Types


### 목록

| 데이터형 | 정의 | 의미 |
|:---|:---|:---|
|BYTE|`typedef unsigned char BYTE;`|바이트(8비트)입니다.|
|BOOL|`typedef int BOOL;`|부울 변수( TRUE 또는 FALSE)|
|CHAR|`typedef char CHAR;`|ANSI(8비트 Windows) 문자|
|COLORREF|`typedef DWORD COLORREF;`|빨강, 녹색, 파랑(RGB) 색 값(32비트)|
|DWORD|`typedef unsigned long DWORD;`|32비트 부호 없는 정수 / 범위는 0부터 4294967295 소수|
|-|-|-|
|PVOID|`typedef void* PVOID;`|'P'는 pointer를 의미/ unsigned int / 모든 형식에 대한 포인터(데이터 형이 없는 메모리 주소를 저장하는 데이터형) / **`핸들에 관련된 데이터형은 PVOID => void* => unsigned int => 32bit 이다.`**|
|HANDLE|`typedef PVOID HANDLE;`|unsigned int / 오브젝트 핸들|
|HDC|`typedef HANDLE HDC;`|'H'는 HANDLE을 의미 / DC( 디바이스 컨텍스트 )에 대한 핸들|
|HGDIOBJ|`typedef HANDLE HGDIOBJ;`|'H'는 HANDLE을 의미 / GDI 개체에 대한 핸들|
|HINSTANCE|`typedef HANDLE HINSTANCE;`|'H'는 HANDLE을 의미 / unsigned int / 인스턴스에 대한 핸들 / 메모리에 있는 모듈의 기본 주소|
|HWND|`typedef HANDLE HWND;`|'H'는 HANDLE을 의미 / unsigned int / 윈도우에 대한 핸들|
|LPVOID|`typedef void* LPVOID;`|모든 형식에 대한 포인터|
|LPWORD|`typedef WORD* LPWORD;`|WORD에 대한 포인터|
|-|-|-|
|LPSTR|`typedef CHAR* LPSTR;`|'STR은 string'ANSI(8비트 Windows) 문자의 null로 끝나는 문자열에 대한 포인터|
|LPWSTR|`typedef WCHAR* LPWSTR;`|'W는 유니코드 의미' 16비트 유니코드 문자의 null로 끝나는 문자열에 대한 포인터|
|LPTSTR|-|유니코드가 정의되면 LPWSTR, 그렇지 않으면 LPSTR|
|TCHAR|-|유니코드가 정의되면 WCHAR이고, 그렇지 않으면 CHAR|
|-|-|-|
|UINT|`typedef unsigned int UINT;`|부호 없는 INT / 범위는 0부터 4294967295 소수|
|VOID|`#define VOID void`|모든 유형|
|WINAPI|`#define WINAPI __stdcall`|시스템 함수에 대한 호출 규칙 / CALLBACK, WINAPI 및 APIENTRY 는 모두 __stdcall 호출 규칙을 사용하여 함수를 정의하는 데 사용된다.|
|WORD|`typedef unsigned short WORD;`|16비트 부호 없는 정수 / 범위는 0에서 65535 10진수|
|WPARAM|`typedef UINT_PTR WPARAM;`|메시지 매개 변수|
|UINT_PTR|``|WIN64 이면 unsigned __int64 UINT_PTR, 아니면 unsigned int UINT_PTR|
|LPARAM|`typedef LONG_PTR LPARAM;`|메시지 매개 변수|
|LONG_PTR|``|WIN64이면 typedef __int64 LONG_PTR, 아니면 typedef long LONG_PTR|
||``||
||``||
||``||
||``||
||``||
















