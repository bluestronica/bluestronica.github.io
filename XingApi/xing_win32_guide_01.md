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
  - 서버에서 jamong으로 메시지(XM_LOGIN) 전송
    - IXingAPI.h에 메시지 정의가 있다.
  - 메시지ID값은 Connect시에 설정한 nStartMsgID와 더하여 사용하면 된다.
- jamong
  - CALLBACK WndProc() 실행
  - iMessage는 서버에서 전송한 WM_APP+XM_LOGIN 값이 된다.
  - 메시지의 wParam 값이 "0000"이면 성공, 그 외에는 실패


#### TR이란
- Transaction의 약자로 서버에 데이터를 요청하고 데이터를 받는 일련의 행동을 일컫는다.
- TR은 조회 데이터용과 실시간 데이터용의 2가지가 존재한다.
  - 조회TR
    - 서버로 데이터 요청 당시의 데이터를 전송
  - 실시간TR
    - 서버로 데이터 요청(ETK_Advise()) 이후에 데이터가 변경될 때마다 데이터를 전송
    - 데이터 요청종료(ETK_Unadvise())를 하면 데이터 전송을 멈춘다.
  - 시세 등 실시간으로 변하는 데이터를 받기위해서는 조회TR을 요청하여 당시의 데이터를 받은 이후에 실시간TR을 요청하여 실시간 데이터를 받아야만 한다.

#### 데이터 조회 과정
- 조회TR 요청(ETK_Request())
- 조회TR에 대한 응답
  - 조회TR 요청 시 등록한 윈도우로 XM_RECEIVE_DATA 메시지 전송
- 조회 결과 데이터 처리
- 실시간 데이터가 필요하면 실시간 등록 이후에 사용되며 사용이 완료된 이후에 실시간 해제 호출
- 실시간 데이터 등록은 수신 받기를 원할 때 등록해도 되지만, 조회 Data를 받은 이후에 하는 것을 추천한다.

#### 단일 데이터(조회 TR) 처리
- jamong에서 조회성TR 함수 호출
- eBest 서버에서 TR처리 후 jamong에 Data 전송
  - Data는 REQUEST_DATA - MESSAGE_DATA - RELEASE_DATA 순으로 오게 된다.
- jamong
  - iMessage의 값은 WM_RECEIVE_DATA = WM_APP + XM_RECEIVE_DATA
  - wParam은 Data 구분 플래그, lParam은 Data 위치를 나타내는 포인터
    - wParam
      - REQUEST_DATA
        - 요청한 Data를 전송
      - MESSAGE_DATA 
        - Data 처리에 대한 Message
        - Data 처리에 성공을 해도 실패를 해도 Message는 발생
        - Message Data를 처리하고 더 이상 사용하지 않을 경우 ETK_ReleaseMessageData()를 호출하여 Message를 해제하여야 한다.
      - SYSTEM_ERROR_DATA
        - Data 처리 이전에 System 문제로 인한 Error가 발생할 경우에 전송한다.
        - Message Data를 처리하고 더 이상 사용하지 않을 경우 ETK_ReleaseMessageData()를 호출하여 Message를 해제하여야 한다.
        - System Error가 발생할 경우 Release가 전송되지 않으며 ETK_ReleaseMessageData()에서 자동으로 RequestID를 해제해 준다.
      - RELEASE_DATA
        - Data 처리가 완료된 경우에 전송
        - RequestID를 해제하기 위해 ETK_ReleaseMessageData()를 호출하여야 한다.
    - lParam
      - 조회TR 수신 Packet
      - wParam이 REQUEST_DATA 일 때,
        - RECV_PACKET 구조체
          - Data 상태를 나타낸다.
          - RequestID(nRqID)는 0~255사이의 값을 사용하며, 해제하지 않으면 255개의 전송이 이루어진 이후에는 전송이 불가능하다.
          - 그래서 수신이 완료가 되면 해제해 주어야 한다.
      - wParam이 MESSAGE_DATA 일 때,
        - MSG_PACKET 구조체








































