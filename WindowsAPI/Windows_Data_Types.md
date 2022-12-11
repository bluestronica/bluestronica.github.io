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
|PVOID|`typedef void* PVOID;`|'P'는 pointer를 의미/ unsigned int / 모든 형식에 대한 포인터(데이터 형이 없는 메모리 주소를 저장하는 데이터형) / ** `핸들에 관련된 데이터형은 PVOID => void* => unsigned int 이다.` **|
|HANDLE|`typedef PVOID HANDLE;`|unsigned int / 오브젝트 핸들|
|HDC|`typedef HANDLE HDC;`|DC( 디바이스 컨텍스트 )에 대한 핸들|
|HGDIOBJ|`typedef HANDLE HGDIOBJ;`|GDI 개체에 대한 핸들|
|HINSTANCE|`typedef HANDLE HINSTANCE;`|unsigned int / 인스턴스에 대한 핸들 / 메모리에 있는 모듈의 기본 주소|
|HWND|`typedef HANDLE HWND;`|unsigned int / 윈도우에 대한 핸들|
|LPVOID|`typedef void* LPVOID;`|모든 형식에 대한 포인터|
|LPWORD|`typedef WORD* LPWORD;`|WORD에 대한 포인터|
||``||
||``||
||``||
||``||
||``||
||``||
||``||


