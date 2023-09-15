## Path
  
### PathMgr
경로를 관리하는 클래스
- Init()
- const wchar_t* GetContentPath()
- wchar_t[] content_path_
  - 절대경로
    - solution 폴더까지 절대경로 값을 얻는다.
  - 상대경로
    - resource가 폴더까지 경로를 지정한다.
  - content_path_
    - 절대경로 + 상대경로


## c-style
- 모든 문자 표현은 c-style로 작성한다.
- const wchar_t*
  - 수정 불가한 문자열
- wchar_t[]
  - 수정 가능한 문자열     

## Resource
