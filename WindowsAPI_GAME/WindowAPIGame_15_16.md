# Scene

### CScene
#### Object를 그룹별로 관리한다.
- 화면에 존재하는 모든 것은 Object이다. 그래서 Object를 일괄적으로 관리해주는 것이 필요하다.
- 하나의 Scene안에 여러 종류의 object들을 그룹별로 관리하기 위해 그룹으로 분류한다.
- 그룹으로 분류된 object들을 `vector<CObject*>`으로 저장하고 관리한다.
- CObject를 포인터로 담는 이유
  - CObject는 앞으로 파생될 모든 클래스들의 부모 클래스가 된다.
  - 그래서 자식 클래스(파생된 클래스)들의 타입들을 부모 클래스 타입으로 받을 수 있기 때문이다.
- `vector<CObject*> marrObj[(UINT)GROUP_TYPE::END];`
  - 배열로 저장된 각각의 그룹타입에는 CObject 포인터들이 벡터에 저장되어 있다.
  - 즉, CObject 포인터들은 벡터에 저장되고 그룹별로 분류되어 그룹타입으로 배열에 저장된다.
  - 그룹별의 값은 enum으로 정리한다.

#### CObject 전방선언
- 전방선언을 하면 CObject에 대한 구체적인 정보를 얻을 수 없다.
- 그래서 포인터 타입으로 선언한다. CObject의 포인터 사이즈는 고정되어 있기 때문이다.
- 전방선언을 하는 이유는 최대한 컴파일 속도에 영향을 주지 않기 위해서 이다.
- 만약 직접 헤더를 참조했다면 CObject 코드가 매번 변경되면 CScene에서 매번 확인을 해야한다.


### CSceneMgr
#### CSceneMgr은 모든 CScene들을 관리한다.
- `CScene* marrScene[(UINT)SCENE_TYPE::END];`
  - 하나의 CScene에 32개의 그룹들로 분류되어 그룹별로 CObject들을 관리한다.
  - 그런 CScene은 여러가지(start, tool, 등등) CScene(SCENE_TYPE)들로 발생하게 되고 그것을 관리하는게 CSceneMgr이다.

#### CScene은 직접 사용할 클래스가 아니라 상속을 시킬 목적의 클래스이다.
- 이걸경우 반드시 CScene에서 소멸자의 가상함수를 무조건 해줘야 한다.
- `virtual ~CScene();`
- CSceneMgr가 모든 파생된 CScene들을 부모 CScene 포인터로 관리하고 있다.
- 관리하던 CScene들을 소멸할 때 다 지워야 한다.
- delete를 하게 되면 전부다 부모 CScene 포인터 타입이기 때문에
- 만약 `virtual ~Cscene()`을 호출을 하지 않으면 부모 CScene만 삭제하게된다.

