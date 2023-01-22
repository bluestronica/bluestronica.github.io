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
- Absolute position of eye and displayed object
![img](IMG/PPS.png)

- Relative position of eye and displayed object
![img](IMG/PPS2.png)
### 시간 동기화
- 컴퓨터는 빠르게 돌고있는 와중에 움직이는 물체들이 우리 현실 시간과 매칭이 되어야한다.
- 초당 100픽셀 이동은 컴퓨터 속도가 아무리 빨라도 초당 100픽셀, 아무리 느려도 초당 100픽셀이 이동한다는 개념 자체가 실제 시간과 매칭된다는 의미이다.
- 