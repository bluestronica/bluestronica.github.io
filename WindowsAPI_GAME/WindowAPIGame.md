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
  - POINT 해상도(1280, 768)
- 윈도우 스타일 기법인 **FAILED 매크로**를 사용해서 수행 성공여부를 체크한다.
  - FAILED 매크로는 0보다 작으면 true를 반환한다.
  - 그래서, FAILED(E_FAIL)는 true를 반환
  - **FAILED(S_OK)는 false 반환**
- 해상도에 맞게 윈도우 크기 조정과 세팅
  - AdjustWindowRect
  - SetWindowPos

#### 6. 게임 실행
- `초기값 -> Progress{ 상태변경 -> 랜더링 }`
- 캐릭터(물체)
  - pos : center
  - scale :  100, 100 
- Progress
  - 상태 변경 처리하고
  - 처리된 데이터로 렌더링한다.
- 물체 상태를 저장하는 개체 생성
  - position
  - 가로, 세로 길이 scale
  - 그 값을 담을 수 있는 float(실수) 타입 x, y를 가지는 구조체 생성(Vec)

### Game - 10 ~ 11 (TimeMgr)
#### 1. 물체를 특정키가 눌렀을 때 포지션 변경
- 키 눌린 순간 바로 체크하는 비동기 키 입출력 함수
  - GetAsyncKeyState
    - 리턴값은 `0, 0x8000, 0x8001, 1` 4가지다.
    - `if (GetAsyncKeyState(VK_LEFT) & 0x8000)`
    - `if (GetAsyncKeyState(VK_RIGHT) & 0x8000)`

#### 2. 프레임 동기화
- 1프레임당 픽셀 이동 거리 = PPS * 1Frame time(DeltaTime)
  - `vPos.x += 10.f;`
  - `vPos.x += 100.f * 1Frame time`
- 1Frame time(DeltaTime)값을 구하기 위한 CTimeMgr 클래스 작성
  - 싱글톤
  - DeltaTime 구하기 구현
    - QueryPerformanceCounter
    - QueryPerformanceFrequency
  - FPS 구하기
    - 1초 동안 프레임 개수 구하기
    - 1초 동안 호출 카운트 

#### 3. 창 제목에 FPS, DeltaTime 나타내기

### Game - 12 (Double Buffering)
#### 1. 이중 버퍼링 용도의 비트맵과 DC를 만든다.
- CreateCompatibleDC
- CreateCompatibleBitmap
- SelectObject
#### 2. 비트맵에 그려놓은 그림을 출력용 DC에 복사하여 화면에 출력
- BitBlt

### Game - 13 ~ 14 (KeyMgr)
### 1. 키 정보를 가지는 struct 작성
- enum class KEY_STATE
- enum class KEY
- struct tKeyInfo
  - KEY_STATE eState
  - bool bPrevPush

### 2. Init
- 벡터안에 키 정보를 채운다.

### 3. Update
- 매번 업데이트마다 모든 키들에 대해서 상태값 체크
- 자신이 설정한 가상 키(KEY enum)와 실제 윈도우 가상 키 값에 매칭 시키는 작업
  - `int garrVK[(int)KEY::LAST]`

### 4. 윈도우 포커싱
- GetFocus
- 윈도우 포커싱 중일 때 키 이벤트 동작
- 윈도우 포커싱 해제 상태일 때 키 상태 변경


