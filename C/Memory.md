[bluestronica.github.io/C](https://bluestronica.github.io/C)


# 레지스터, 스택 & 힙, 동적메모리, 다중 포인터


### 레지스터(register)
- 레지스터는 CPU가 사용하는 저장 공간 중에 가장 빠른 저장 공간
  - 레지스터는 흔히 말하는 '메모리'가 아니다.
  - CPU가 레지스터에 접근하는 방법과 메모리 접근하는 방벙이 다른 게 보통
- CPU가 연산을 할 때 보통 레지스터에 저장되어 있는 데이터를 사용
- 그 연산결과도 레지스터에 다시 저장하는 게 보통

### x86 아키텍쳐에서 사용하는 레지스터
- 8개의 범용 레지스터
  - ESP, EBP, EAX, EBX, ECX, EDX 등등
- 6개의 세그먼트 레지스터
- 1개의 플래그 레지스터
- 1개의 명령 포인터
- 등등
![img](Img/assem.png)
![img](Img/register1.png)