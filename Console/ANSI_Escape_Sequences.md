[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# ANSI Escape Sequences
### Sequences
- `ESC` - sequence starting with `ESC` (`\x1B`)
- `CSI` - Control Sequence Introducer: sequence starting with `ESC[` or CSI (`\x9B`)
- `DCS` - Device Control String: sequence starting with `ESCP` or DCS (`\x90`)
- `OSC` - Operating System Command: sequence starting with `ESC]` or OSC (`\x9D`)

### Cursor Controls
- #define CSI "ESC["

|ESC Code Sequence|Example|Description|
|:---|:---|:---|
|`ESC[H`|`printf(CSI, "H");`|(0, 0) 위치로 커서 이동|
|`ESC[{line};{column}H`|`printf(CSI, "4;5H");`|moves cursor to line #, column #|
|`ESC[{line};{column}f`|`printf(CSI, "%d;%df", line, column);`|moves cursor to line #, column #|