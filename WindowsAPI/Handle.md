[bluestronica.github.io/WindowsAPI](https://bluestronica.github.io/WindowsAPI)


# HANDLE


### Handle
- 운영체제는 리소스를 안전하게 관리하기 위해서 직접적으로 주소를 사용하는 포이터 대신 Handle이라는 개념을 사용한다.
- **Handle이란 운영체제 내부에 있는 어떤 리소스의 주소를 정수(32bit 혹은 64bit)로 치환한 값이다.**
- 따라서, Handle이란 개체(object)의 주소를 나타내는 자료형(data type)이라 생각할 수 있다.

### Handle의 특징
- Handle은 구분을 위한 것이므로 중복을 허용하지 않는다.
- 운영체제가 발급하며 사용자는 사용만 한다.
- 'H'로 시작하는 접두어를 가진다.
  - HCURSOR(마우스 커서), HICON(아이콘), HWND(윈도우)
- 운영체제는 Handle Table에 리소스의 주소와 Handle의 값을 저장해서 관리하고 있다.
