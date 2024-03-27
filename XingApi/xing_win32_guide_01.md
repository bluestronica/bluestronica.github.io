#### eBest + xingApi + Win32 + Visual Studio
- 프로젝트
  - jamong
  - 스스로 꿈을 이루다.

#### eBest + xingApi
- 계좌개설
- API사용등록
- xingApi다운로드 + 설치
- 샘플다운로드

#### Win32 + Visual Studio
- Windows 데스크톱 마법사
  - jamong_solution\jamong_prject\ 
- 데스크톱 애플리케이션(exe)
- 빈 프로젝트(E)

#### jamong 기본 세팅
- C:\eBEST\xingAPI
  - 모든 DLL 파일과 Sys 폴더를 복사
- C:\jamong_solution\Debug
  - 복사한 DLL 파일과 Sys 폴더를 붙여넣기
- C:\eBEST\xingAPI
  - IXingAPI.h 복사
- C:\jamong_solution\jamong_project\
  - IXingApI.h 붙여넣기
  - packet 폴더 생성
    - 조회TR/실시간TR 코드 별로 구조체 정의가 있는 .h파일을 정리한 폴더
  - C:\sample\
    - Helper.h 복사
    - Helper.cpp 복사
  - C:\jamong_solution\jamong_project\
    - Helper.h 붙여넣기
    - Helper.cpp 붙여넣기
- jamong_project 속성
  - 고급
  - 문자 집합
    - 멀티바이트 문자 집합 사용

#### xingApi 초기화
- xing.Init()
  - XingAPI.dll 파일 로드 시도
  - NULL 이면 실패

#### xingApi 연결
- xing.Connect()
  - 서버에서 jamong으로 보낼 Message의 sTartMsgID 값은 WM_APP로 한다.
- xing.Connected()
- xing.Disconnect()

#### xingApi 로그인
- jamong에서 로그인 요청
  - xing.Login()
- eBest 서버에서 로그인 처리
  - 서버에서 메시지(XM_LOGIN) 전송
    - IXingAPI.h에 메시지 정의가 있다.
  - 메시지ID값은 Connect시에 설정한 nStartMsgID와 더하여 사용하면 된다.
- jamong
  - CALLBACK WndProc() 실행
  - iMessage는 서버에서 전송한 WM_APP+XM_LOGIN 값이 된다.
  - 메시지의 wParam 값이 "0000"이면 성공, 그 외에는 실패













































