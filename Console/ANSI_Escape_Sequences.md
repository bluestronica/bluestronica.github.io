[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# ANSI Escape Sequences
## Sequences
- `ESC` - sequence starting with `ESC` (`\x1B`)
- `CSI` - Control Sequence Introducer: sequence starting with `ESC[` or CSI (`\x9B`)
- `DCS` - Device Control String: sequence starting with `ESCP` or DCS (`\x90`)
- `OSC` - Operating System Command: sequence starting with `ESC]` or OSC (`\x9D`)

## Cursor Controls
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


## Erase Funcions
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


## Colors / Graphics Mode
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
|`ESC[38;2;{r};{g};{b}m`|Set foreground color as RGB.|`printf(CSI, "38;2;5;77;120m");`|
|`ESC[48;2;{r};{g};{b}m`|Set background color as RGB.|`printf(CSI, "48;2;3;44;100m");`|    



## Screen Modes
- `#define CSI "ESC["`

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[={value}h`|화면의 width, type을 값으로 지정된 모드로 변경한다.|`printf("ESC[=0h");`|
|`ESC[=0h`|40 x 25 흑백(monochrome) (text)|`printf(CSI, "=0h");`|
|`ESC[=1h`|40 x 25 칼라(color) (text)|`printf(CSI, "=1h");`|
|`ESC[=2h`|80 x 25 흑백(monochrome) (text)|`printf(CSI, "=2h");`|
|`ESC[=3h`|80 x 25 칼라(color) (text)|`printf(CSI, "=3h");`|
|`ESC[=4h`|320 x 200 4-color (graphics) (text)|`printf(CSI, "=4h");`|
|`ESC[=5h`|320 x 200 monochrome (graphics) (text)|`printf(CSI, "=5h");`|
|`ESC[=6h`|640 x 200 monochrome (graphics) (text)|`printf(CSI, "=6h");`|
|`ESC[=7h`|줄 바꿈 활성화|`printf(CSI, "=7h");`|
|`ESC[=13h`|320 x 200 color (graphics)|`printf(CSI, "=13h");`|
|`ESC[=14h`|640 x 200 color (16-color graphics)|`printf(CSI, "=14h");`|
|`ESC[=15h`|640 x 350 monochrome (2-color graphics)|`printf(CSI, "=15h");`|
|`ESC[=16h`|640 x 350 color (16-color graphics)|`printf(CSI, "=16h");`|
|`ESC[=17h`|640 x 480 monochrome (2-color graphics)|`printf(CSI, "=17h");`|
|`ESC[=18h`|640 x 480 color (16-color graphics)|`printf(CSI, "=18h");`|
|`ESC[=19h`|320 x 200 color (256-color graphics)|`printf(CSI, "=19h");`|
|`ESC[={value}l`|설정 모드에서 사용하는 동일한 값을 사용하여 모드를 재설정한다.|`printf("ESC[=0l");`|


- Common Private Modes
    - `#define CSI "ESC["`
    - l은 L의 소문자   

|ESC Code Sequence|Description|Example|
|:---|:---|:---|
|`ESC[?25l`|커서를 보이지 않게 하기|`printf(CSI, "?25l");`|
|`ESC[?25h`|커서를 보이게 하기|`printf(CSI, "?25h");`|
|`ESC[?47l`|화면 복원|`printf(CSI, "?471");`|
|`ESC[?47h`|화면 저장|`printf(CSI, "?47h");`|
|`ESC[?1049h`|대체 버퍼 활성화|`printf(CSI, "?1049h");`|
|`ESC[?1049l`|대체 버퍼 비활성화|`printf(CSI, "?10491");`|




## Keyboard Strings
- 키보드 키를 지정된 문자열로 재정의한다.
- `ESC[{code};{string};{...}p`
    - `code`는 아래 표에 나열된 값 중 하나 이상이다.
    - `string`은 단일 문자에 대한 ASCII 코드 이거나
    - 따옴표로 묶인 문자열이다.
    - 예를 들어, **65**와 **"A"**는 모두 **대문자 A**를 나타내는 데 사용할 수 있다.

|Key|Code|SHIFT+code|CTRL+code|ALT+code|
|:---|:---|:---|:---|:---|
|F1|0;59|0;84|0;94|0;104|
|F2|0;60|0;85|0;95|0;105|
|F3|0;61|0;86|0;96|0;106|
|F4|0;62|0;87|0;97|0;107|
|F5|0;63|0;88|0;98|0;108|
|F6|0;64|0;89|0;99|0;109|
|F7|0;65|0;90|0;100|0;110|
|F8|0;66|0;91|0;101|0;111|
|F9|0;67|0;92|0;102|0;112|
|F10|0;68|0;93|0;103|0;113|
|F11|0;133|0;135|0;137|0;139|
|F12|0;134|0;136|0;138|0;140|
|-|-|-|-|-|
|HOME (num keypad)|0;71|55|0;119|--|
|UP ARROW (num keypad)|0;72|56|(0;141)|--|
|PAGE UP (num keypad)|0;73|57|0;132|--|
|LEFT ARROW (num keypad)|0;75|52|0;115|--|
|RIGHT ARROW (num keypad)|0;77|54|0;116|--|
|END (num keypad)|0;79|49|0;117|--|
|DOWN ARROW (num keypad)|0;80|50|(0;145)|--|
|PAGE DOWN (num keypad)|0;81|51|0;118|--|
|INSERT (num keypad)|0;82|48|(0;146)|--|
|DELETE (num keypad)|0;83|46|(0;147)|--|
|-|-|-|-|-|
|HOME|224;71|224;71|224;119|(224;151)|
|UP ARROW|224;72|224;72|(224;141)|(224;152)|
|PAGE UP|224;73|224;73|224;132|(224;153)|
|LEFT ARROW|224;75|224;75|224;115|(224;155)|
|RIGHT ARROW|224;77|224;77|224;116|(224;157)|
|END|224;79|224;79|224;117|(224;159)|
|DOWN ARROW|224;80|224;80|(224;145)|(224;154)|
|PAGE DOWN|224;81|224;81|224;118|(224;161)|
|INSERT|224;82|224;82|(224;146)|(224;162)|
|DELETE|224;83|224;83|(224;147)|(224;163)|
|-|-|-|-|-|
|PRINT SCREEN|--|--|0;144|--|
|PAUSE/BREAK|--|--|0;0|--|
|BACKSPACE|8|8|127|(0)|
|ENTER|13|--|10|(0)|
|TAB|9|0;15|(0;148)|(0;165)|
|NULL|0;3|--|--|--|
|-|-|-|-|-|
|A|97|65|1|0;30|
|B|98|66|2|0;48|
|C|99|67|3|0;46|
|D|100|68|4|0;32|
|E|101|69|5|0;18|
|F|102|70|6|0;33|
|G|103|71|7|0;34|
|H|104|72|8|0;35|
|I|105|73|9|0;23|
|J|106|74|10|0;36|
|K|107|75|11|0;37|
|L|108|76|12|0;38|
|M|109|77|13|0;50|
|N|110|78|14|0;49|
|O|111|79|15|0;24|
|P|112|80|16|0;25|
|Q|113|81|17|0;16|
|R|114|82|18|0;19|
|S|115|83|19|0;31|
|T|116|84|20|0;20|
|U|117|85|21|0;22|
|V|118|86|22|0;47|
|W|119|87|23|0;17|
|X|120|88|24|0;45|
|Y|121|89|25|0;21|
|Z|122|90|26|0;44|
|-|-|-|-|-|
|1|49|33|--|0;120|
|2|50|64|0|0;121|
|3|51|35|--|0;122|
|4|52|36|--|0;123|
|5|53|37|--|0;124|
|6|54|94|30|0;125|
|7|55|38|--|0;126|
|8|56|42|--|0;127|
|9|57|40|--|0;128|
|0|48|41|--|0;129|
|-|-|-|-|-|
|-|45|95|31|0;130|
|=|61|43|--|0;131|
|[|91|123|27|0;26|
|]|93|125|29|0;27|
||92|124|28|0;43|
|;|59|58|--|0;39|
|*`*|39|34|--|0;40|
|,|44|60|--|0;51|
|.|46|62|--|0;52|
|/|47|63|--|0;53|
|`|96|126|--|(0;41)|
|-|-|-|-|-|
|ENTER (keypad)|13|--|10|(0;166)|
|/ (keypad)|47|47|(0;142)|(0;74)|
|* (keypad)|42|(0;144)|(0;78)|--|
|- (keypad)|45|45|(0;149)|(0;164)|
|+ (keypad)|43|43|(0;150)|(0;55)|
|5 (keypad)|(0;76)|53|(0;143)|--|
