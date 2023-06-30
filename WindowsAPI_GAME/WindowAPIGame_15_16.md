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
- **그래서 비용이 들지 않는다.**
- 거의 인터페이스같은 역할
- CScene.h
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

- CScene.cpp
```C++
#include "pch.h"
#include "CScene.h"

#include "CObject.h"

CScene::CScene()
{
}

CScene::~CScene()
{
	for (UINT i = 0; i < (UINT)GROUP_TYPE::END; ++i)
	{
		for (size_t j = 0; j < marrObj[i].size(); ++i)
		{
			// marrObj[i] 그룹 벡터의 j CObject 삭제
			delete marrObj[i][j];

// CObject를 담고 있던 벡터는 
// CScene의 맴버이기 때문에 CScene 소멸자에서 삭제된다.
// 자연스럽게 CScene 소멸하면 CScene이 소유하고 있던 맴버들도 소멸한다.
// 지금 여기 보이지는 않지만 소멸할때 자동으로 호출되어 삭제한다. 
// CObject를 따로 루프돌려서 삭제한 이유는
// 벡터에 들어간게 각각의 CObject* 포인터였고 포인터가 가리키는 주소에 가보면 
// 그기에 동적할당된 공간이 있다. 
// 그래서 벡터만 지워서는 동적할당된 공간은 삭제가 안된다.
		}
	}
}

void CScene::Update()
{
	for (UINT i = 0; i < (UINT)GROUP_TYPE::END; ++i)
	{
		for (size_t j = 0; j < marrObj[i].size(); ++i)
		{
			marrObj[i][j]->Update();
		}
	}
}

void CScene::Render(HDC hdc)
{
	for (UINT i = 0; i < (UINT)GROUP_TYPE::END; ++i)
	{
		for (size_t j = 0; j < marrObj[i].size(); ++i)
		{
			marrObj[i][j]->Render(hdc);
		}
	}
}
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

```C++
#pragma once

class CScene;

class CSceneMgr
{
	SINGLE(CSceneMgr);

private:
	CScene* marrScene[(UINT)SCENE_TYPE::END];  // 모든 CScene 목록
	CScene* mpCurScene;  // 현재 CScene

public:
	void Init();
	void Update();
	void Render(HDC hdc);
};
```

### CScene_Start
#### CScene을 상속 받은 CScene_Start
- CScene_Start.h
```C++
#pragma once
#include "CScene.h"

// CScene을 상속 받은 CScene_Start
class CScene_Start :
    public CScene
{
public:
    // virtual 안적어줘도 되지만 구분할 수 있게 명시적으로 적어주자!
    virtual void Enter(); 
    virtual void Exit();

public:
    CScene_Start();
    ~CScene_Start();  // 자식 소멸자부터 삭제된다.
};
```

- CScene_Start.cpp
```C++
#include "pch.h"
#include "CScene_Start.h"

#include "CObject.h"

CScene_Start::CScene_Start()
{
}

CScene_Start::~CScene_Start()
{
}

void CScene_Start::Enter()
{
	// Object 추가
	CObject* pObj = new CObject;

	pObj->SetPos(Vec(640.f, 384.f));
	pObj->SetScale(Vec(100.f, 100.f));

//marrObj[(UINT)GROUP_TYPE::DEFAULT].push_back(pObj); 
// marrObj은 부모클래스의 private 맴버이기 때문에 자식클래스에서는 접근이 안된다.
// 그래서 상속받은 자식이 부모에게 접근 하기위해 private -> protected로 변경한다.
// 그러나 이러한 private 맴버 변수에 바로 접근해서 변경하는 것 보다
// protected: 함수로 넘겨주는게 더 좋다.
// 에러 났을 경우를 대비해서

	AddObject(pObj, GROUP_TYPE::DEFAULT);  // 인라인 함수로 호출
}

void CScene_Start::Exit()
{

}
``
