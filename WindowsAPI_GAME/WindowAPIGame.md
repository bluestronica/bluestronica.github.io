# 진행 순서

### Game_01_06 (기본 구조)
- main.cpp
- g_hWnd 전역변수 만들기
- 메시지 루프 변경(PeekMessageA)
- red 1t 실선, 브러쉬 blue 사각형 그리기
- pos  500, 300 / scale 100, 100
- 키보드 방향키로 사각형 이동하기
- 마우스 클릭했을 때, 무브, 클릭놓았을때, 사각형 크기 변경
- 사각형을 vector에 저장해서 계속 추가 그리기
- GetTickCount()을 이용한 1초가 흐른 시간동안 PeekMessageA에서 처리한 누적 시간 비율 값을 윈도우창 제목에 출력


### Game_07_09 (Singleton / Core Class)
#### 1. main.cpp
- g_hWnd 
- PeekMessageA

#### 2. 폴더, 파일 정리
- Default
- Engine
  - Header
  - Core
    - 게임 프로그램 생성과 유지
- main.cpp
  - 윈도우 프로그램 생성과 관리
- pch.h

#### 3. 미리 컴파일된 헤더
*모든 cpp 파일들은 pch.h 파일을 참조해야 한다. **클래스 만들 때마다 자동으로 추가 된다.***
- pch.h
  - `#include "define.h"`
- 속성 설정
  - 만들기(/Yc), pch.h

#### 4. Singleton Pattern을 위한 매크로 함수 만들기
*미션 : 싱글톤 패턴을 이해하고 구현할 수 있도록 습득*
- 동적할당 싱글톤
- **데이터섹션 싱글톤 구현**
  - Engine/Core/CCore.h
- 매크로 함수 만들기  
  - Engine/Header/define.h

#### 5. CCore 인스턴스 초기화
- 초기화 함수 매개변수
  - HWND 핸들, 
  - POINT 해상도
- 윈도우 스타일 기법인 **FAILED 매크로**를 사용해서 수행 성공여부를 체크한다.
  - FAILED 매크로는 0보다 작으면 true를 반환한다.
  - 그래서, FAILED(E_FAIL)는 true를 반환
  - **FAILED(S_OK)는 false 반환**
- 해상도에 맞게 윈도우 크기 조정과 세팅
  - AdjustWindowRect
  - SetWindowPos

#### 6. 게임 실행
- Progress
  - 데이터를 업데이트하고
  - 업데이트한 데이터로 렌더링한다.

























