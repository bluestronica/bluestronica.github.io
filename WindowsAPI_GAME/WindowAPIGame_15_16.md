# Scene

### CScene
#### CObject를 그룹별로 관리한다.
- 화면에 존재하는 모든 것은 CObject이다. 그래서 CObject를 일괄적으로 관리해주는 것이 필요하다.
- 하나의 Scene안에 여러 종류의 CObject들을 그룹별로 관리하기 위해 그룹으로 분류한다.
- 그룹으로 분류된 CObject들을 `vector<CObject*>`으로 저장하고 관리한다.
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

#### 순수 가상함수
- virtual은 부모에 선언하고 각각 자식들만의 기능은 Enter()를 만들어서 실행
- `virtual void Enter() = 0`
  - 순수 가상함수는 기능을 만들필요없다. 하지만 자식 Enter()는 반드시 구현해야한다. 강제시킨다.

#### 인라인 함수
- private 맴버변수를 자식들에게만 노출되게 protected:로 범위 지정했다.
- 하지만 그보다는 인라인 함수를 만들어 노출하는게 더 낫다.
- 클래스는 헤더에 함수 작성하면 인라인 처리된다.
- 인라인으로 호출되면 호출된 스택에서 바로 해결한다.
- ** 그래서 비용이 안든다.**
- 거의 인터페이스같은 역할

```C++
#pragma once

// CScene에서 CObject를 그룹별로 관리할 것이다.
// 최대 그룹값을 enum으로 정리할 것이다.

// 전방선언
class CObject;

class CScene
{
private:
	vector<CObject*> marrObj[(UINT)GROUP_TYPE::END];
	wstring mstrName; // Scene 이름

public:
	void SetName(const wstring& strName) { mstrName = strName; }
	const wstring& GetName() { return mstrName; }

	void Update();
	void Render(HDC hdc);

	virtual void Enter() = 0; // 해당 Scene에 진입 시 호출
	virtual void Exit() = 0;  // 해당 Scene을 찰출 시 호출

protected:
	void AddObject(CObject* pObj, GROUP_TYPE eTyp)
	{
		marrObj[(UINT)eTyp].push_back(pObj);
	}

public:
	CScene();
	virtual ~CScene();  // 상속된 자식 CScene을 삭제하기위해
};
```


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


### CScene_Start
#### CScene_Start.h
- CScene을 상속 받은 CScene_Start
```C++
class CScene_Start :
    public CScene
{
public:
    CScene_Start();
    ~CScene_Start();  // 자식 소멸자부터 삭제된다
    
};
```
