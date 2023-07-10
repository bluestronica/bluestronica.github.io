# CObject

### CObject
- CObject.h
```C++
class CObject
{
private:
	Vec v_pos;
	Vec v_scale;

public:
	virtual void Update() = 0; // 순수 가상함수,
                                   // 모든 자식 클래스는 자기만의 Update를 구현해야한다.
	virtual void Render(HDC memDc);

public:
	void SetPos(Vec pos)
	{
		v_pos = pos;
	}

	void SetScale(Vec scale)
	{
		v_scale = scale;
	}

	Vec GetPos()
	{
		return v_pos;
	}

	Vec GetScale()
	{
		return v_scale;
	}

public:
	CObject();
	~CObject();
};
```

- CObject.cpp
```C++
#include "pch.h"
#include "CObject.h"
#include "CKeyMgr.h"
#include "CTimeMgr.h"

CObject::CObject()
	: v_pos{}
	, v_scale{}
{
}

CObject::~CObject()
{
}

void CObject::Render(HDC memDc)
{
	Rectangle(memDc
		, (int)v_pos.x - (int)v_scale.y / 2
		, (int)v_pos.y - (int)v_scale.y / 2
		, (int)(v_pos.x + v_scale.y / 2.f)
		, (int)(v_pos.y + v_scale.y / 2.f));
}
```

### CPlayer
- CPlayer.h
```C++
#pragma once
#include "CObject.h"

class CPlayer :
    public CObject
{
public:
    virtual void Update();

private:
    void createMissile();
};
```

- CPlayer.cpp
```C++
#include "pch.h"
#include "CPlayer.h"
#include "CSceneMgr.h"
#include "CScene.h"

#include "CKeyMgr.h"
#include "CTimeMgr.h"
#include "CMissile.h"

void CPlayer::Update()
{
	Vec pos = GetPos();

	if (KEY_CHECK(EKey::LEFT, EKeyState::HOLD))
	{
		pos.x -= 200.f * fDT;
	}

	if (KEY_HOLD(EKey::RIGHT))
	{
		pos.x += 200.f * fDT;
	}

	if (CKeyMgr::GetIns()->GetKeyStateByEKey(EKey::UP) == EKeyState::HOLD)
	{
		pos.y -= 200.f * fDT;
	}

	if (CKeyMgr::GetIns()->GetKeyStateByEKey(EKey::DOWN) == EKeyState::HOLD)
	{
		pos.y += 200.f * fDT;
	}

	if (KEY_TAP(EKey::SPACE))
	{
		createMissile();
	}

	SetPos(pos);
}

void CPlayer::createMissile()
{
	Vec missilePos = GetPos();
	missilePos.y -= GetScale().y / 2.f;

	// Missile Object
	CMissile* missile = new CMissile;
	missile->SetPos(missilePos);
	missile->SetScale(Vec(25.f, 25.f));
	missile->SetDir(true);

	CScene* curScene = CSceneMgr::GetIns()->GetCurScene();
	curScene->AddObj(missile, EGroupByObject::Default);
}
```

### CMonster
- CMonster.h
```C++
#pragma once
#include "CObject.h"
class CMonster :
    public CObject
{
private:
    Vec vec_centerPos;   // monster 위치 중앙점
    float f_speed;
    float f_maxDistance;
    int i_dir;  // 진행 방향 1, -1
    

public:
    float GetSpeed() { return f_speed; }
    void SetSpeed(float speed) { f_speed = speed; }
    void SetMoveDistance(float moveDis) { f_maxDistance = moveDis; }
    void SetCenterPos(Vec pos) { vec_centerPos = pos; }

public:
    virtual void Update();

public:
    CMonster();
    ~CMonster();
};
```

- CMonster.cpp
```C++
#include "pch.h"
#include "CMonster.h"
#include "CTimeMgr.h"

CMonster::CMonster()
	: vec_centerPos(Vec(0.f, 0.f))
	, f_speed(100.f)
	, f_maxDistance(50.f)
	, i_dir(1)
{
}

CMonster::~CMonster()
{
}

void CMonster::Update()
{
	Vec curPos = GetPos();

	// 진향 방향으로 시간당 f_speed 만큼 이동
	curPos.x += fDT * f_speed * (float)i_dir;

	// 배회 거리 기준량을 넘어섰는지 확인
	float dist = abs(vec_centerPos.x - curPos.x) - f_maxDistance;

	if (0.f < dist)
	{
		i_dir *= -1;
		curPos.x += dist * i_dir;
	}

	SetPos(curPos);
}
```


### CMissile
- CMissile.h
```C++
#pragma once
#include "CObject.h"
class CMissile :
    public CObject
{
private:
    float f_dir; // 위아래 미사일 방향

public:
    void SetDir(bool bUp)
    {
        if (bUp)
        {
            f_dir = -1.f;
        }
        else
        {
            f_dir = 1.f;
        }
    }
    
public:
    virtual void Update();
    virtual void Render(HDC memDc);

public:
    CMissile();
    ~CMissile();
};
```

- CMissile.cpp
```C++
#include "pch.h"
#include "CMissile.h"
#include "CTimeMgr.h"

CMissile::CMissile()
	: f_dir(1.f)
{
}

CMissile::~CMissile()
{
}

void CMissile::Update()
{
	Vec pos = GetPos();
	pos.y += 400.f * fDT * f_dir;
	SetPos(pos);
}

void CMissile::Render(HDC memDc)
{
	Vec pos = GetPos();
	Vec scale = GetScale();

	Ellipse(memDc
		, (int)pos.x - (int)scale.y / 2
		, (int)pos.y - (int)scale.y / 2
		, (int)(pos.x + scale.y / 2.f)
		, (int)(pos.y + scale.y / 2.f));
}
```
