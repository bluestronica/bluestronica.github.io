[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# GDI, GDI 오브젝트, DC의 개념
- GDI는 화면, 프린터와 같은 모든 출력 장치를 제어하는 인터페이스이다.
- 그래픽을 출력하기 위해 사용되는 도구를 GDI Object라고 한다.
- 펜, 브러시, 비트맵, 폰트 등이 GDI Object에 해당된다.
- 이러한 GDI Object는 DC(Device Context)에 담겨져 있다.
- DC에서 원하는 내용을 변경하여 출력한다.
- 그래서 여태까지 GetDC, BeginPaint와 같은 함수를 사용해 DC를 생성해 핸들을 얻어서 사용했던 것이다.
![img](Img/DC_GDI.png)
- GDI Object는 DC에 담겨저 있고 GDI를 통해 출력 장치에 출력을 하게 된다.
- 개발자는 GetDC, BeginPaint와 같은 함수를 통해 DC를 생성하게 된다.
- GDI Object는 내부는 공개되어 있지 않기 때문에, 핸들을 이용해서 사용한다.
- 기본적으로 DC가 만들어졌을 때 디폴트로 선택된 GDI Object는 다음과 같다.

| GDI Object | 핸들 타입 | 설명 | 디폴트 |
|:---|:---|:---|:---|
|펜(Pen)|HPEN|선을 그을 때 사용된다.|검정색의 가는 선|
|브러시(Brush)|HBRUSH|면을 채울 때 사용된다.|흰색|
|폰트(Font)|HFONT|문자 출력에 사용되는 글꼴|시스템 글꼴|
|비트맵(HBITMAP)|HBITMAP|비트맵 이미지|선택되지 않음|
|팔레트(HPALETTE)|HPALETTE|팔레트|선택되지 않음|
|영역(RGN)|HRGN|영역|선택되지 않음|

### 스톡 오브젝트(Stock Object)
