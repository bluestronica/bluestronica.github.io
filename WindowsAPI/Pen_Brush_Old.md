[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)

# 개요
- 펜과 브러쉬를 활용해 화면에 도형을 출력할 것이다. 
- 기본적으로 GDI Object를 만들어 사용하는 원리는 다음과 같다.
```c
HPEN MyPen, OldPen;         // 1.핸들을 선언한다.

MyPen = CreatePen(~);       // 2.GDI 오브젝트를 생성한다.

OldPen = SelectObject(~);   // 3.OldPen에 이전에 사용하던 핸들을 저장한다.

Rectangle, Ellipse ~        // 4.GDI 오브젝트를 사용한다.

SelectObject(hdc, OldPen);  // 5.선택을 해제한다.

DelectObject(MyPen);        // 6.메모리에서 삭제한다.
```
- 위와 같은 과정을 거쳐 Rectangle을 이용해 사각형을 그리게 된다면 펜을 생성할 때 설정한 것이 적용되어 사각형이 그려진다.
- 펜의 역할은 테두리를 그리는 것이므로 사각형의 테두리가 변경된다. 
- 펜을 활용해 사각형을 그리는 과정을 보면, 핸들을 선언하고, GDI Object를 생성한 다음, OldPen에 이전에 사용하던 오브젝트를 저장한다. 
- 이후, 사용 후 선택을 해제하고 메모리에서 삭제하는 것을 볼 수 있다.

# 펜(Pen)
- Windows API에서 제공하는 스톡 펜(Stock Pen)은 흰색, 검은색, 투명색이다.
- 다른 색상의 펜은 개발자가 직접 만들어서 사용해야 한다. 
- 새로운 펜을 만들기 위한 함수는 CreatePen 함수이다.
- **`HPEN CreatePen(int fnPenStyle, int nWidth, COLORREF crColor);`**
  - 만들어지는 펜의 핸들을 반환하며, 
  - 인수로는 펜으로 그려질 선의 종류, 굵기, 색상이 들어간다.
  - 색상은 RGB(R,G,B)를 이용해 넣어주면 된다.
  - 선의 종류는 PS_SOLID(실선), PS_DOT(점선) 등이 있다.

### GDI_Blue_Pen 프로젝트
- 펜을 생성해 파란색 테두리를 가진 사각형 출력
```c

```



















