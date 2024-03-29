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
#### 1. 윈도우 프로그램 생성과 관리를 하는 main Class
- ghWnd 
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

#### 3. 미리 컴파일된 헤더 설정 
*모든 cpp 파일들은 pch.h 파일을 참조해야 한다. **클래스 만들 때마다 자동으로 추가 된다.***
- pch.h
  - `#include "define.h"`
- 속성 설정
  - 만들기(/Yc), pch.h

#### 4. Game 프로그램 생성과 관리를 하는 Core Class
- **생성 : GetInst()**
  - 동적할당 싱글톤
  - **데이터섹션 싱글톤 구현**
  - 매크로 함수 만들기  
    - Engine/Header/define.h

- **초기화 : Init()**
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

- **실행 - Progress()**
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
  - Update()
  - Render()


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
#### 1. 키 정보를 가지는 struct 작성
- enum class KEY_STATE
- enum class KEY
- struct tKeyInfo
  - KEY_STATE eState
  - bool bPrevPush

#### 2. Init
- 벡터안에 키 정보를 채운다.

#### 3. Update
- 매번 업데이트마다 모든 키들에 대해서 상태값 체크
- 자신이 설정한 가상 키(KEY enum)와 실제 윈도우 가상 키 값에 매칭 시키는 작업
  - `int garrVK[(int)KEY::LAST]`

#### 4. 윈도우 포커싱
- GetFocus
- 윈도우 포커싱 중일 때 키 이벤트 동작
- 윈도우 포커싱 해제 상태일 때 키 상태 변경


### Game - 15 ~ 16 (SceneMgr)
#### 1. enum class
- ESceneType
  - Scene의 종류
  - (시작화면, 툴 화면, 등등)
- EGroupByObject
  - Scene에 존재하는 모든 Object들을 Scene type별로 Object를 그룹화한다.
  - ( Default, Monster, 등등)

#### 2. CSceneMgr
- arr_p_scenes
- p_currScene->(CStartScene)Enter();
  - 게임 프로그램이 시작할 보여주는 화면
  - CStartScene 강제 설정
  - Enter()
    - CStartScne에 나타나는 모든 Object를 등록한다.


#### 3. 부모 CScene
- arr_v_objs
- inline 함수
  - AddObj() : Object를 등록하는 함수
- virtual Enter()
- virtual Exit()

#### 4. 자식 CScene 
- Enter()
  - Object 등록
- Exit()

#### 5. CObject
- Update()
- Render()

#### 6. 소멸자 작성
- arr_p_scenes
- arr_v_objs


### Game - 17 ~ 18 (Object)
#### 1.Object
- Update, Render
  - 화면에 보여줄 objet는 전부 다르게 그려야 한다.
    
#### 2. Monster
- 좌우 무브
  - 이동거리 넘는 순간 방향전환  
- 경계선 처리
- 다수의 몬스터 나타내기
  - 나타낼 몬스터간의 간격은 (cnt - 1)

#### 3. Palyer

#### 4. Missile
- 일회성 소모용 class
- CPlayer->Update() 에서 Missile이 생성되어 소모된다.



























