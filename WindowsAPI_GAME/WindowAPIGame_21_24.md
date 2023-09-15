## Path
  
### PathMgr
경로를 관리하는 클래스
- Init()
- const wchar_t* GetContentPath()
- wchar_t content_path_[255]
  - solution 폴더까지 절대경로 값을 얻는다.
  - solution에서 resource가 있는 폴더까지 경로를 지정한다.
  - content_path_
    - solution 경로 + resource 경로


## c-style
모든 문자 표현은 c-style로 작성한다.
```C++
// 수정 불가한 문자 : 데이타 섹션에 저장
const wchar_t* sz = L"\\bin\\content\\";

// 수정 가능한 문자열 : 스택에 저장
wchar_t dest[255] = {0, };
wchar_t source[] = L"\\bin\\content\\";
size_t source_length = wcsnlen_s(source, 255);

wcsncat_s(dest, 255, source , source_length);
```


## Resource
