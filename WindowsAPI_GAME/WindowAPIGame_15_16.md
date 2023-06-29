# Scene
- 화면에 존재하는 모든 것은 Object이다. 그래서 Object를 일괄적으로 관리해주는 것이 필요하다.

### CScene
#### Object를 그룹별로 관리한다.
- 하나의 Scene안에 여러 종류의 object들을 그룹별로 관리하기 위해 그룹으로 분류한다.
- 그룹으로 분류된 object들을 `vector<CObject*>`으로 저장하고 관리한다.
- CObject를 포인터로 담는 이유
  - CObject는 앞으로 파생될 모든 클래스들의 부모 클래스가 된다.
  - 그래서 자식 클래스(파생된 클래스)들의 타입들을 부모 클래스 타입으로 받을 수 있기 때문이다.
- `vector<CObject*> marrObj[(UINT)GROUP_TYPE::END];`
  - 배열로 저장된 각각의 그룹타입에는 CObject 포인터들이 벡터에 저장되어 있다.
  - 즉, CObject 포인터들은 벡터에 저장되고 그룹별로 분류되어 그룹타입으로 배열에 저장된다.

