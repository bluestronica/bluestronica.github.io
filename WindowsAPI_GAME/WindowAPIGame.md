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
- `초기값 -> Progress{ 상태변경 -> 랜더링 }`
- 물체에 초기값 부여
  - pos : center
  - scale :  100, 100 
- Progress
  - 상태 변경 처리하고
  - 처리된 데이터로 렌더링한다.
- 물체 상태를 저장하는 개체 생성
  - position
  - 가로, 세로 길이 scale
  - 그 값을 담을 수 있는 float(실수) 타입 x, y를 가지는 구조체 생성(Vec)

### Game - 10 ~ 11 (Time)
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
  - 어떤 DC와 호환(Compatible) 되는 DC를 만드는 방식이다.
  - 매개 변수로 전달된 hdc와 호환이 된다라는 뜻은 hdc가 사용하는 출력 장치의 종류나
  - 출력장치가 사용 중인 그래픽 드라이버 정보를 가지고 있는 새로운 DC를 만든다는 뜻이다.
  - 따라서 이렇게 만들어진 DC는 DC에 연결된 비트맵에 그림을 그릴 때 hdc와 동인한 방법으로 그림을 그리게 된다.
  - CreateCompatibleDC를 사용해서 얻은 DC는 출력 대상이 없는 상태로 그리기 특성만 정해져서 만들어지기 때문에 비트맵 객체가 연결은 되어 있지만 제대로 된 비트맵 객체는 아니다.
  - CreateCompatibleDC 함수로 만든 DC에 연결된 비트맵 객체 정보를 얻어보면 1비트 색상에 폭과 높이가 1인 비트맵이라는 것을 확인할 수 있다. 
  - 결국 사용하라고 연결한 비트맵 객체가 아니라 그냥 DC를 구성하기 위해서 연결한 비트맵이라는 것이다.
  - 따라서 CreateCompatibleDC로 생성한 DC를 사용하려면 비트맵 객체를 만들어서 연결하는 작업을 먼저 해야한다.
- CreateCompatibleBitmap
  - 비트맵을 만든다.
- SelectObject
  - 메모리 DC와 비트맵 객체 연결
#### 2. 비트맵에 그려놓은 그림을 출력용 DC에 복사하여 화면에 출력
- BitBlt





