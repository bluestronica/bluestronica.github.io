# Double Buffering
- 더블 버퍼링은 화면에 표시해야 할 내용을 미리 메모리에 모두 그려놓은 후 화면으로 한꺼번에 전송하는 방법을 말한다.
- 즉, 어떠한 내용을 그릴 때, 매번 화면으로 출력을 보내는 것이 아니라, 화면으로 내보일 내용을 미리 메모리에 모두 그려 놓은 후 그것을 한꺼번에 화면으로 딱 한 번 그려내자는 것이다. 
- Double Buffering은 copy와 flip의 2가지 기법이 있다.

### Double Buffering - copy(BitBlt)
![img](IMG/DoubleBuffering.jpg)

- 이중 버퍼링 용도의 비트맵과 DC를 만든다.
```c++
m_hBit = CreateCompatibleBitmap(m_hDC, m_ptResolution.x, m_ptResolution.y);
m_memDC = CreateCompatibleDC(m_hDC);

// m_hBit와 m_memDC 연결
HBITMAP hOldBit = (HBITMAP)SelectObject(m_memDC, m_hBit);
DeleteObject(hOldBit);
```
- 메모리에 그려 놓은 후 실제 화면에 복사 옮겨 그리기 
```c++
Rectangle(m_memDC, -1, -1, m_ptResolution.x + 1, m_ptResolution.y + 1);

// 그리기
Vec2 vPos = g_obj.GetPos();
Vec2 vScale = g_obj.GetScale();

// m_memDC와 연결된 m_hBit에 사각형을 그린다. 이 그림은 화면에 출력되지 않는다.
Rectangle(m_memDC, int(vPos.x - vScale.x / 2.f),
         int(vPos.y - vScale.y / 2.f),
         int(vPos.x + vScale.x / 2.f),
         int(vPos.y + vScale.y / 2.f));

// Memory DC와 화면 출력용 DC를 사용하여 비트맵에 그려진 그림을 복사하여 화면에 출력
BitBlt(m_hDC, 0, 0, m_ptResolution.x, m_ptResolution.y
  , m_memDC, 0, 0, SRCCOPY);
```
