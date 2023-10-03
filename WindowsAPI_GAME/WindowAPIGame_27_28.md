## Collider
  
### Object를 적절한 그룹에 배정
- 1대1로 모든 물체가 충돌하는지 전부 검사하면 검사량 횟수가 엄청나게 늘어나게 된다.
- 그것을 방지하고 충돌을 효율적으로 다루기위해서 1대1이 아닌 그룹끼리 충돌하는지 검사하면 된다.
- 그렇기 때문에 명확하게 Object의 용도와 속성에 따라 그룹을 분류하고 적절한 그룹에 배정해서 넣어줘야 한다.
- `Player -> EObjectsGroupby::PALAYER`
- `Monster -> EObjectsGroupby::MONSTER`

### 충돌체 지정
- Start Scene에 진입(Enter)하고 Object 생성 등록 후 마지막에 충돌체을 지정한다.

### 충돌 검사
- 물체들이 업데이트를 하고 그 다음 파이널 업데이트까지 한 후 충돌검사를하고 랜더링이 되어야 한다.
- 즉, Core에서 SceneMgr의 Update가 끝난 후 CollisionMgr의 Update가 실행 된 후 랜더링으로 넘어간다.
- 그리고 어떤 그룹이랑 어떤 그룹들이 충돌해줄지를 해당 Scene이 Enter진입하고 맨 마지막에 지정을 해준다.
- 그 다음 충돌 검사한다. 업데이트 돌 때 충돌 검사도 진행이 된다.
- CollisionMgr
  - 그룹간의 충돌에 대한 체크를 관리하는 Manager
  - 총 32개의 그룹 - 조합 (31X31)
    - 중복 그룹은 제외한다.
    - 그러므로 반쪽만 있으면 된다.
    - 비트로 관리 : 있다없다, on_off로 체크
    - 4바이트 부호없는 정수가 비트 32개(하나의 그룹이 32개의 그룹에 일대일 대응)
      - (M x N)
  - `size_t collision_checks_[(size_t)EObjectsGroupby::END];`
  - `CheckCollision(EObjectsGroupby left_group_type, EObjectsGroupby right_group_type)`
    - CheckCollision 함수는 지정한 그룹을 알려주면 그룹의 비트자리를 체크해준다.



### 충돌 진행

### 충돌체 간의 이전 프레임 충돌 정보


### 충돌 해제
