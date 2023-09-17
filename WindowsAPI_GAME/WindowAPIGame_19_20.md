# WindowsApiGame_01_20


#### main.cpp
#### Default
#### Engine


#### 기본 폴더 작성
main.cpp
- PeekMessage()
- memory leak check
  - `#include <crtdbg.h>` 
  - `_CrtSetDbgFlag(_CRTDBG_ALLOC_MEM_DF | _CRTDBG_LEAK_CHECK_DF)`
  - `_CrtSetBreakAlloc(138)`
- Engine/Core
- Engine/Default


#### 메인 윈도우창 세팅
Core : Core Class
- Create
  - SINGLE()
- Init
  - check -> FAILED()
  - hwnd, POINT
  - adjust_window 
- Progress
  - Update
  - Render


#### 화면 중앙에 object 그리기
Object Class
- pos
- scale

Vec struct
- x
- y

Render()
- Rectangle()


#### object 움직이기
- Update()
- Manager
  - TimeMgr
    - 1프레임당 이동거리
    - speed * deltatime
- dubble buffering
  - mem_dc_handle
  - compatible_bitmap_handle
  - BitBlt
- Manager
  - KeyMgr
    - enum class Ekey
    - enum class KeyState
      -  키가 눌렸을 여부에 따라,
      - 키상태 변경
    - struct KeyInfo
    - Vector<KeyInfo> keys_
    - Keys sync with VKs
      -(asynckeystate(vk) & 0x8000)
    - etFocus


#### 각각의 화면에는 화면에 나타나는 모든 Object를 가진다.
- Manager
  - SceneMgr
    - enum class SceneType
    - CScene* scenes_[]
- Scene
  - base
    - void update()
    - void render()
    - virtual void Enter() = 0
    - virtual void Exit() = 0
  - derived
    - virtual void Enter() override
      - object 등록
    - virtual void exit() override
    - object 삭제
- Object
  - base
    - virtual void update() = 0
    - virtual void render() = 0
  - derived
    - virtual void update() override
    - virtual void render() override
- Player : public Object
- Monster : public Object
  - auto left-right move
    - start_pos
    - moving_distance
      - monster scale.x / 2
    - if (|start_pos - pos.x| > moving_distnace)
    - turn direction
      - x * 1
      - x * -1
  - 5 monster
    - monster_cnt
    - start_position
      - (screen_resolution.x - (max_mvoing *2) * 2) / (monster_cnt - 1)
      - for( ; i < monster_cnt; ; )
    - SetStartPos
- Missile : public Object
  - CreateMissile()


#### 내가 원하는 방향으로 미사일 쏘기
- 라디안
  - 각을 실수로 표현하는 방법
  - 호의 길이가 반지름과 같으면 1, 57도
    - 2배 면 , 114도
    - pi배 면 180도
    - 고로 pi = 180도
    - 호의 길이가 반지름의 3.14배면 180도
- 삼각함수
  - 단위 원
    - 반지름이 1,
    - 원점(0, 0)을 중심으로
    - 갖는 원을 단위 원이라 부른다.
  - 단위 원과 세타(θ)를 타나내는 동경과의 교점인
    - x 좌표를 cosθ
    - y 좌표를 sinθ
    - (cosθ, sinθ)
  - 기울기는
    - sinθ / cosθ
    - y / x	
- 벡터
  - 크기와 방향을 동시에 나타내는 벡터
  - 각도 방식을 벡터 방식으로
    - 물체를 이동 시킬 때 이동 방향을
    - 각도 방식으로 쓰는 것 보다
    - 조금 더 접근성이 쉽고 직관적인 방법 중의
    - 하나인 벡터를 사용한다.
  - 벡터의 내적
    - a*b = |a||b|cosθ
  - 단위 벡터
    - 방향을 가지는 크기가 1인 벡터
  - 정규화(Normalize)
    - 방향과 크기를 가진 어떤 벡터를 크기가 1인 벡터 즉, 단위벡터로 만드는 것을 말한다.
    - 좌표 x, y 가 있다면,
    - 벡터의 크기 c = sqrt(x*x + y*y)
    - c = 1 로 만들기(정규화) 위해서
    - x / c , y / c가 된다.
  - 벡터를 단위벡터로 만드(Normalize)는 이유
    - 물체의 이동 거리에 크기(길이) 값이
    - 들어가면 이동 거리에 영향을 받는다.
    - 그래서 방향은 같고 크기(길이)가 1인
    - 순수 방향을 나타내는 단위 벡터를 사용한다.

#### Direction
- **theta(θ)**
  - PI = 180
    - θ를 실수로 표현하기 위해 radian을 사용한다.
  - 방향
    - x_direction
      - cosθ
    - y_direction
      - sinθ
  - 크기
    - position.x
    - position.y
  - **크기와 방향을 가진 theta**
    - position.x * cosθ
    - position.y * sinθ
  
- **vector**
  - 어떤 크기와 방향을 가진 벡터를 단위백터로 만든다.
  - 방향
    - 단위벡터
      - 정규화 과정을 거쳐서 만든다.
      - 단위벡터는 순수 방향만 가진다.
  - 크기
    - position.x
    - position.y
  - **크기와 방향을 가진 vector**
    - position.x * unit_vector.x
    - position.y * unit_vector.y
   






