# Timer

### FPS(Frame Per Second) - 초당 프레임 수
![img](IMG/FPS.png)

| FPS | Frame time(s) | Frame time(ms) |
|:---|:---|:---|
|20|1/20|50ms|
|30|1/30|33.33ms|
|60|1/60|16.67ms|
|100|1/100|10ms|

### PPS(Pixels Per Second) - 초당 픽셀들 수
- 다양한 프레임 속도(frame rate)에 대한 다음 그래프 세트에서 초당 1920픽셀의 속도(PPS)로 시간에 눈의 위치와 표시된 개체를 플로팅했다.

- Absolute position of eye and displayed object
![img](IMG/PPS1.png)

  - 보시다시피 물체의 표시된 위치는 선형 이동에 근사치만 적용하고 프레임 속도(frame rate)가 높을수록 근사치가 더 좋아진다. 눈과 표시된 개체의 위치는 프레임당 한 번만 일치한다. 표시된 물체와 눈의 상대적 위치(즉, 사람의 시야에서 물체의 인지된 위치)를 플로팅하면 다음 그래프를 얻을 수 있다.

- Relative position of eye and displayed object
![img](IMG/PPS2.png)

  - 표시된 물체는 시야에서 주기적으로 앞뒤로 움직인다. 이 움직임은 진동(vibration)으로 설명될 수 있으므로 지금부터 이 용어를 사용한다. 이 진동(vibration)의 진폭(amplitude)은 프레임 속도(frame rate)가 증가함에 따라 감소하고 주파수(frequency)(1초 동안에 진동하는 수)는 프레임 속도와 동일하다는 것을 알 수 있다.

### 시간 동기화 하기
- 초당 1920픽셀 이동은 컴퓨터 속도가 아무리 빨라도 초당 1920픽셀, 아무리 느려도 초당 1920픽셀이 이동한다는 개념 자체가 실제 시간과 매칭된다는 의미이다.
- **1프레임당 픽셀 이동 거리 = PPS * 1Frame time**
- 1Frame time은 컴퓨터의 성능에 따라 달라질 수 있기 때문에 매번 1Frame time을 얻어와서 **1프레임당 픽셀 이동 거리**를 계산해낸다.
- 그리고 그것을 1초 동안 반복 합산하면 초당 1920픽셀의 총 이동량이 된다.

### CTimeMgr
```C++
class CTimeMgr
{
	SINGLE(CTimeMgr);
private:
	LARGE_INTEGER	m_llCurCount;  // QueryPerformanceCounter로 부터 얻는 현재 카운트 값(백만단위)
	LARGE_INTEGER	m_llPrevCount; // 이전 카운트
	LARGE_INTEGER	m_llFrequency; // 1초가 지난 그 사이 카운트를 세어 나타낸 값

	double			m_dDT;	// 프레임 사이의 시간 값
	double			m_dAcc;  // 1초 체크를 위한 누적 시간
	UINT			m_iCallCount;	// 함수 호출 횟수 체크
	UINT			m_iFPS;			// 초당 호출 횟수


	// FPS : 1초동안 프레임 개수를 알면 1 프레임당 시간을 알 수 있다.
	// 1 프레임당 시간 , 아주 미세한 시간 데이터 타입, (Delta Time)


	// 1초에 천식 세는 겟틱카운트함수 이걸로 카운트에 현재 카운트와 
  // 일정시간 지난뒤 카운트를 또 체크해서
	// 벌어진 벌어진 차이 값을 천으로 나눠서 실제 현실 시간이 얼마나 
  // 흘렸는지를 체크해 줄 수 있었다.
	// 

public:
	void init();
	void update();  // 매 프레임마다 호출하는 함수

public:
	double GetDT() { return m_dDT; }
	float GetfDT() { return (float)m_dDT; }
};
```































