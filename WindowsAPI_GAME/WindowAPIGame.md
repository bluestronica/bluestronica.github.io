# 진행 순서

### Game_01_06 (기본 구조)
  1. main.cpp
  2. g_hWnd 전역변수 만들기
  3. 메시지 루프 변경(PeekMessageA)
  4. red 1t 실선, 브러쉬 blue 사각형 그리기
  5. pos  500, 300 / scale 100, 100
  6. 키보드 방향키로 사각형 이동하기
  7. 마우스 클릭했을 때, 무브, 클릭놓았을때, 사각형 크기 변경
  8. 사각형을 vector에 저장해서 계속 추가 그리기
  9. GetTickCount()을 이용한 1초가 흐른 시간동안 PeekMessageA에서 처리한 누적 시간 비율 값을 윈도우창 제목에 출력

### Game_07_09 (Singleton / Core Class)