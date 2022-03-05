[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# ANSI Escape Sequences
### Sequences
- `ESC` - sequence starting with `ESC` (`\x1B`)
- `CSI` - Control Sequence Introducer: sequence starting with `ESC[` or CSI (`\x9B`)
- `DCS` - Device Control String: sequence starting with `ESCP` or DCS (`\x90`)
- `OSC` - Operating System Command: sequence starting with `ESC]` or OSC (`\x9D`)

### Cursor Controls
- `#define CSI "ESC["`
- `#`은 매개 변수
    - 이동할 거리를 나타내며 선택적 매개변수
    - 생략되거나 0이면 1로 처리된다.
    - 음수일 수 없다.

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[H`|(0, 0) 위치로 커서 이동|`printf(CSI, "H");`|
|`ESC[{line};{column}H`|moves cursor to line #, column #|`printf(CSI, "4;5H");`|
|`ESC[{line};{column}f`|moves cursor to line #, column #|`printf(CSI, "%d;%df", line, column);`|
|-|-|-|
|`ESC[#A`|# 라인만큼 커서를 UP|`printf(CSI, "%dA", line);`|
|`ESC[#B`|# 라인만큼 커서를 DOWN|`printf("ESC[%dB", down);`|
|`ESC[#C`|# 라인만큼 커서를 RIGHT|`printf("ESC[%5C");`|
|`ESC[#D`|# 라인만큼 커서를 LEFT|`printf(CSI, "7D");`|
|-|-|-|
|`ESC[#E`|# 라인 아래(DOWN) 라인의 시작 부분으로 커서 이동|`printf(CSI, "%dE", line);`|
|`ESC[#F`|# 라인 이전(UP) 라인의 시작 부분으로 커서 이동|`printf(CSI, "%dF", line);`|
|`ESC[M`|커서를 한 라인 위로 이동하고 필요한 경우 스크롤한다.|`printf(CSI, "M");`|
|-|-|-|
|`ESC[#G`|# 열(column)로 커서 이동|`printf(CSI, "%dG, column);`|
|-|-|-|
|`ESC[6n`|커서 위치 보고 : reports as `ESC[#;#R`|`printf(CSI, "6n");`|
|-|-|-|
|`ESC[7`|커서 위치 저장|`printf(CSI, "7");`|
|`ESC[8`|커서를 마지막으로 저장된 위치로 복원|`printf(CSI, "8");`|


### Erase Funcions
- `#define CSI "ESC["`
- 줄을 지워도 커서가 이동하지 않는다.
- 즉, 커서는 줄이 지워지기 전의 마지막 위치에 유지된다.
- 줄을 지운 후 `\r`을 사용하여 커서를 현재 줄의 시작 부분으로 되돌릴 수 있다.

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[0J`|현재 커서 위치에서 화면 끝까지|`printf(CSI, "0J");`|
|`ESC[1J`|현재 커서 위치에서 화면 시작까지|`printf(CSI, "1J");`|
|`ESC[2J`|전체 화면 지우기|`printf("ESC[2J");`|
|-|-|-|
|`ESC[0K`|현재 커서 위치에서 라인 끝까지|`printf(CSI, "0K");`|
|`ESC[1K`|현재 커서 위치에서 라인 시작까지|`printf(CSI, "0K");`|
|`ESC[2K`|라인 전체 지우기|`printf(CSI, "0K");`|


### Colors / Graphics Mode
- `#define CSI "ESC["`

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[1;34;{...}m`|셀의 그래픽 모드를 `;`으로 구분하여 설정|`printf(CSI, "1;34m");`|
|`ESC[0m`|모든 모드 **재설정**(스타일 및 색상)|`printf(CSI, "0m");`|
|`ESC[1m`|**굵은(Bold)** 모드를 설정|`printf(CSI, "%dm", mode);`|
|`ESC[2m`|**희미한(dim)/약한(faint)** 모드를 설정|`printf("ESC[2m");`|
|`ESC[3m`|**기울임(italic)** 모드를 설정|`printf("ESC[3m");`|
|`ESC[4m`|**밑줄(underline)** 모드를 설정|`printf(CSI, "4m");`|
|`ESC[5m`|**깜빡임(blinking)** 모드를 설정|`printf(CSI, "5m");`|
|`ESC[7m`|**반전(inverse/reverse)** 모드를 설정|`printf(CSI, "7m");`|
|`ESC[8m`|**숨김(hidden/invisible)** 모드를 설정|`printf(CSI, "8m");`|
|`ESC[9m`|**취소선(strikethrough)** 모드를 설정|`printf(CSI, "9m");`|


- 8-16 Colors  

|Color Name|Foreground Color Code|Background Color Code|
|:---|:---|:---|
|Black|30|40|
|Red|31|41|
|Green|32|42|
|Yellow|33|43|
|Blue|34|44|
|Magenta|35|45|
|Cyan|36|46|
|White|37|47|
|Default|39|49|
|Reset|0|0|

```c++
# Set style to bold, red foreground.
printf(CSI, "1;31m");
printf("Hello");

# Set style to dimmed, white foreground with red background.
printf(CSI, "2;37;41m");
printf("World");
````

|Color Name|Foreground Color Code|Background Color Code|
|:---|:---|:---|
|Bright Black|90|100|
|Bright Red|91|101|
|Bright /Green|92|102|
|Bright Yellow|93|103|
|Bright Blue|94|104|
|Bright Magenta|95|105|
|Bright Cyan|96|106|
|Bright White|97|107|


- 256 Colors
    - `;38`과 `;48`은 16색 시퀀스에 해당하며
    - 터미널에서 해석하여 전경색과 배경색을 각각 설정한다.ㅕ
    - `;5`는 색상 형식을 설정한다.
    - {ID}는 색상표의 0에서 255사이의 색상 인덱스로 대체 되어야 한다.   

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[38;5;{ID}m`|Set foreground color.|`printf(CSI, "38;5;1m");`|
|`ESC[48;5;{ID}m`|Set background color.|`printf(CSI, "48;5;256m");`|


- RGB Colors
    - 최신 터미널은 RGB를 사용하여 전경색과 배경색을 설정할 수 있는
    - Truecolor(24-bit RGB)를 지원한다.   
    - `;38`과 `;48`은 16색 시퀀스에 해당하며
    - 터미널에서 해석하여 전경색과 배경색을 각각 설정한다.
    - `;2`는 색상 형식을 설정한다.

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[38;2;{r};{g};{b}m`|Set foreground color as RGB.|`printf(CSI, "38;5;1m");`|
|`ESC[48;2;{r};{g};{b}m`|Set background color as RGB.|`printf(CSI, "48;5;256m");`|    



### Screen Modes
- `#define CSI "ESC["`