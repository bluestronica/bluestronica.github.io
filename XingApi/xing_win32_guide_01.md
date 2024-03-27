### eBest + xingApi + win32 + visual studio

#### eBest + xingApi
- 계좌개설
- API사용등록
- xingApi다운로드 + 설치
- 샘플다운로드

#### win32 + visual studio
- Windows 데스크톱 마법사
  - jamong_solution\jamong_prject\ 
- 데스크톱 애플리케이션(exe)
- 빈 프로젝트(E)

#### DLL 세팅
- C:\eBEST\xingAPI
  - 모든 DLL 파일과 Sys 폴더를 복사
- C:\jamong_solution\Debug
  - win32로 만들어진 프로그램에서 실행(exe) 파일이 있는 폴더(Debug)에 붙여넣기
- C:\eBEST\xingAPI
  - IXingAPI.h 복사
- C:\jamong_solution\jamong_project\
  - IXingApI.h 붙여넣기
  - packet 폴더 생성
- jamong_project 속성
  - 고급
  - 문자 집합
    - 멀티바이트 문자 집합 사용
