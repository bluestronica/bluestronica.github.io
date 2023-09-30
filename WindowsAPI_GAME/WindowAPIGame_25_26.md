## Collider
  
### 물체와 물체끼리 충돌 구현
- 서루 부딪쳤다는 것을 알 수 있어야한다.
  - 부딪치면 데미지를 준다, 밀면 문이 열린다. 포탈위에 있으면 다른 곳으로 워프 등등
  - 이렇듯 상호작용에 촉발점(트리거)으로 어떤 충돌을 그 조건으로 삼는 경우가 많다.
- 조건이 충돌하면(if)이라면 부딪치는 상황에서 프레임마다 데미지를 주면 안된다. 왜냐하면 느린 컴퓨터와 빠른 컴퓨터마다 데미지가 들어가는 값이 달라진다. 그래서 조건을 충돌하면(if) 뿐만 아니라 좀 더 디테일한 조건들이 있어야 한다.
- 충돌하는 시점에서부터 충돌을 유지(Maintain)하는 동안에는 한번으로 생각해야한다. 그래서 충돌에서 필요한 이벤트는 2개의 충돌체가 충돌하는 그때 그 순간 알림이 한번 가야한다.
- 즉, 충돌을 진입한 그 순간(충돌 진입 이벤트 중)
  - 진입(Enter)
  - 여기에 필요한 조건을 건다.
- 두 물체가 계속 충돌 상태면(Maintan) 충돌 진입 이벤트가 아니며 두 물체가 충돌을 벗아는 순간 그 프레임에 이벤트(Exit)가 발생한다.

### 충돌 이벤트 3종류
3 종류의 이벤트를 관리해서 적절한 시점에 함수로 알림을 콜백해준다.
- 두 물체가 충돌했을 때 (Enter)
- 두 물체가 충돌을 지속적으로 유지 중일 때 (Maintain),
- 충돌이 벗어 났을 때 (Exit)

### Component 기반으로 하는 구조 설계
- 결국 충돌을 진행할 수 있는 것들은 Object들이다.
- 충돌이 필요한 object, 충돌이 필요없는 object
- 충돌 기능이 필요하면 활성화 시키고, 필요없으면 비활성화 시킬 수 있는 자유롭게 확장될 수 있는 구조적 설계도 매우 중요하다.
- 오직 상속으로만 설계하는 것은 문제가 생길 여지가 많다. 특히 상속의 깊이가 길어지면 더욱 그렇다.
- 상속을 최소화하고 확장성이 있는 구조를 선택하 때 주로 방법이 Component 기반 구조이다.

### Collider
- Component/Collider/Collider.cpp
- Collider를 Component 기반으로 설계
  - 최상위 부모 클래스 Object는 Collider클래스를 멤버로 가진다.
    - 모든 Object들은 Collider*를 멤버로 가지게 된다.
    - 필요하면 동적할당해서 Collider*를 가진다.
    - 필요없으면 nullptr
  - Collider 생성과 제거는 Object가 책임진다.
    - Player->CreateCollier
    - Monster->CreateCollider
    - Object->~Object
- Collider는 Object에 달라붙어서 그 Object에 바운딩 박스 즉 Object의 충돌 구현을 위한 어떤 영역을 검사한다.
  - Object가 움직일 때, Collider도 같이 따라 움직여야 한다.
  - 그래서 Collider도 자신을 생성한 Object의 정보를 가지고 있어야 한다.
    - ownder_
    - friend class Object
  - 그렇게 함으로써 Collider와 Object는 서로 쌍방 연결이 된 것이다.
- Engin 구조상 매 Update마다(프레임마다, deltatime 시간동안) 필수적으로 해야하는 일들이 있다.
  - 프레임마다 Object가 업데이트 한번씩 돌고 나면 Collider도 업데이트 한번씩 돌아야 한다.
- FinalUpdate의 임무는 Collider가 Player를 따라가게 하는 일
  - 모든 물체들이 다 업데이트가 되면 충돌체(collider)를 보유하고 있는 오브젝트(object)들은 충돌체가 오브젝트 위치로 따라갈 수 있게 마무리 지어주는 작업을 한다.
  - 그리고 나서 랜더링이 되기 때문에 내 눈에서는 오브젝트와 충돌체가 분리된 느낌을 절대 받을 수 없다.
  - SinMgr->scene->finalUpdat(collider::funalUpdate)
- Collider FinalUpdat를 위한 Member 구성
  - offset
    - 자체 포지션을 가지기 위해 Object의 포지션을 중심으로 상대적인 거리 값을 구한다.
  - position
  - scale
    - 이번 프로젝트에서는 오브젝트 scale 변경이 없기때문에 사이즈를 절대값으로 한다.
- 구체적으로 충돌이 제대로 진행되는지를 시각적으로 표현해 줄 필요가 있다.
  - 초종 릴리즈 버전에서 기능을 끄면 된다.
  - 그래서 Colldier에 랜더링 구현 함수를 만든다.
- Colldier::Render
  - Rectangle
  - Win32에서 매 랜더링마다 Brush, Pen 생성과 선택과 다시 되돌려주는 이런 과정이 너무 비효율적이다.
  - 그래서 Core에서 Brush, Pen을 미리 생성
- 그리는 과정을 추상화
  - Module/SelejctGDI/SelectGDI.cpp
  - SelectGDI(memory_dc, EBrushType::Hollow);
  - SelectGDI(memory_dc, EPenType::Green);




































