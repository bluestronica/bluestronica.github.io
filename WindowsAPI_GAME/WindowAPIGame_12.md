# Double Buffering
- 더블 버퍼링은 화면에 표시해야 할 내용을 미리 메모리에 모두 그려놓은 후 화면으로 한꺼번에 전송하는 방법을 말한다.
- 즉, 어떠한 내용을 그릴 때, 매번 화면으로 출력을 보내는 것이 아니라, 화면으로 내보일 내용을 미리 메모리에 모두 그려 놓은 후 그것을 한꺼번에 홤녀으로 딱 한 번 그려내자는 것이다. 
- Double Buffering은 copy와 flip의 2가지 기법이 있다.

### Double Buffering 기법 copy
![img](IMG/DoubleBuffering.jpg)
