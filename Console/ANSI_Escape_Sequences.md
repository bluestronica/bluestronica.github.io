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

|ESC Code Sequence|Example|Description|
|:---|:---|:---|
|`ESC[H`|`printf(CSI, "H");`|(0, 0) 위치로 커서 이동|
|`ESC[{line};{column}H`|`printf(CSI, "4;5H");`|moves cursor to line #, column #|
|`ESC[{line};{column}f`|`printf(CSI, "%d;%df", line, column);`|moves cursor to line #, column #|
|-|-|-|
|`ESC[#A`|`printf(CSI, "%dA", line);`|# 라인만큼 커서를 UP|
|`ESC[#B`|`printf(CSI, "%dB", down);`|# 라인만큼 커서를 DOWN|
|`ESC[#C`|`printf(CSI, "%dC", right);`|# 라인만큼 커서를 RIGHT|
|`ESC[#D`|`printf(CSI, "%dD", left);`|# 라인만큼 커서를 LEFT|
|-|-|-|
|`ESC[#E`|`printf(CSI, "%dE", line);`|# 라인만큼 커서를 UP|
