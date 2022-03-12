[bluestronica.github.io/C](https://bluestronica.github.io/C)

# getchar(), getche(), getch()

### 차이점
|비교|getchar()|getche()|getch()|
|---|---|---|---|
|버퍼 사용|O|X|X|
|화면 표시|O|O|X|
|종료 인식|\n|\r|\r|
|#include <>|stdio.h|conio.h|conio.h|

### getchar()
- 입력 버퍼를 사용하므로, 
- 엔터가 입력될 때까지 입력을 계속 받아 버퍼에 담아둔다.
- 엔터가 들어오면 종료하고
- 버퍼중에서 가장 첫번째 글자의 아스키코드가 반환된다.
    - 키보드로 '7'을 입력했다면 7의 아스키코드(0x37)가 반환될거임
```c
#include <stdio.h>

int c;

// 키보드로 '7'를 입력 후 엔터키 누름('\n')
c = getchar();  // 버퍼로부터 한 글자를 읽어옴 ; 7만 읽어옴
                // 키보드로 '7'을 입력했다면 7의 아스키코드(0x37)가 반환된다.
                // c = 0x37;
while (c != EOF) 
{
    putchar(c);      // 읽은 문자 출력
    c = getchar();   // 아직 버퍼에는 '\n'이 남아 있음
}     
```

### getch()
- 입력 버퍼를 사용하지 않으므로
- 엔터를 입력할 때 까지 기다리지 않는다.
- 키보드를 눌렀다가 떼는 동시에 그 값을 가져가 버린다.
- 키보드에서 '7'을 눌렸지만 
- **getch()는 입력 값을 화면에 보여주지 않는다.**
```c
#include <stdio.h>
#include <conio.h>

int c;

// 키보드로 '7'를 입력 후 엔터키 누름('\n')
c = getch();  // '7'에서 손을 떼는 순간 바로 c에 7의 아스키코드(0x37)가 반환된다.
while (c != EOF) 
{
    putchar(c);      // 읽은 문자 출력
    c = getchar();   
}     
```

### getche()
- getche()는 getch()와 동일하지만
- **입력 값을 화면에 출력**하는 차이가 있다.


# putchar(), putch()

### 차이점
|비교|putchar()|putch()|
|---|---|---|---|
|버퍼 사용|O|X|
|#include <>|stdio.h|conio.h|

### putchar()
- **문자**를 **stdout**에 출력
- **`fputc(ch, stdout)`** 하고 같음

### putch()
- 출력 버퍼를 사용하지 않고
- 문자 하나를 콘솔(표준 출력)에 바로 출력한다.