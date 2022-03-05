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
|`ESC[#B`|# 라인만큼 커서를 DOWN|`printf(CSI, "%dB", down);`|
|`ESC[#C`|# 라인만큼 커서를 RIGHT|`printf(CSI, "%dC", right);`|
|`ESC[#D`|# 라인만큼 커서를 LEFT|`printf(CSI, "%dD", left);`|
|-|-|-|
|`ESC[#E`|# 라인만큼 커서를 UP|`printf(CSI, "%dE", line);`|
