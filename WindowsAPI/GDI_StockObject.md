[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# GDI, GDI 오브젝트, DC의 개념
- GDI는 화면, 프린터와 같은 모든 출력 장치를 제어하는 인터페이스이다.
- 그래픽을 출력하기 위해 사용되는 도구를 GDI Object라고 한다.
- 펜, 브러시, 비트맵, 폰트 등이 GDI Object에 해당된다.
- 이러한 GDI Object는 DC(Device Context)에 담겨져 있다.
- DC에서 원하는 내용을 변경하여 출력한다.
- 그래서 여태까지 GetDC, BeginPaint와 같은 함수를 사용해 DC를 생성해 핸들을 얻어서 사용했던 것이다.
