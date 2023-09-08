## SBCS, MBCS, WBCS 

### SBCS 기반 프로그래밍
- 1바이트 사용
- 아스키코드도 SBCS 문자 형태


### MBCS - multibyte-character set
- 한글은 2바이트, 영어는 1바이트 사용하는 형태
- 마이크로소프트의 독자적인 character set 이라고 봐도 무방하다.


### WBCS 기반의 프로그래밍
- 모든 문자를 2바이트로 사용
- 대표적으로 유니코드가 WBCS 문자 형태
- 유니코드 기반으로 프로그래밍 하는 것이 안정적이고 좋다.
- 유니코드로 프로그래밍을 시작했으면 끝까지 유니코드로 하는 것이 좋다.


### SBCS, MBCS, WBCS 지원하는 함수들

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
