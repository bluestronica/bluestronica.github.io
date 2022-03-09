[bluestronica.github.io/Console](https://bluestronica.github.io/Console)

# Definitions
- Microsoft의 현재 콘솔/터미널은 
- Windows 플랫폼의 개발자에게 직접 최고 수준의 터미널 환경을 제공하는 **클래식 Windows Console API**를 단계적으로 폐지하고, 
- pseudoconsole을 활용하여 **가상 터미널 시퀀스**로 대체하는 것이다.

### Definitions
- **Command Line Applications**
    - **Console Application** 또는 **clients** 이라고 한다.
    - 주로 텍스트 또는 문자 정보 스트림에서 작동하는 프로그램을 뜻한다.
    ###
    - 명령줄 애플리케이션(Command Line Applications)은 
    - 사용자의 키보드 입력을 나타내는 표준 입력 STDIN 핸들에서 텍스트 스트림을 수신하고, 해당 정보를 처리한 다음,
    - 표준 출력 STDOUT 에 있는 텍스트 스트림으로 응답하여 사용자의 모니터에 다시 표시한다.

- **Standard Handles**
    - 표준 핸들(Standard Handles)은 일련의 **STDIN**, **STDOUT** 핸들이며 **STDERR**은 시작 시 프로세스 공간의 일부로 도입된다.
    - 명령줄 애플리케이션의 경우 애플리케이션이 시작될 때 항상 존재해야 한다.

- **Clients and Servers**
    - **clients** : 정보 처리 및 명령 실행 작업을 수행하는 애플리케이션으로 지칭
    - **Servers** : 사용자 인터페이스를 담당하는 클라이언트를 대신하여 입력 및 출력을 표준 양식으로 변환하는 작업자

- **Console Subsystem**
    - 콘솔 및 명령줄 작업에 영향을 주는 모든 모듈을 나타내는 catch-all 용어이다. 
    - 특히 시작 애플리케이션이 명령줄/콘솔 애플리케이션인지(시작하려면 표준 핸들이 있어야 하는지) 
    - 또는 Windows 애플리케이션(필요하지 않음)인지를 지정하는 이식 가능한 실행 파일 헤더의 일부인 플래그를 나타낸다.
    - 콘솔 호스트, 명령줄 클라이언트 애플리케이션, 콘솔 드라이버, 콘솔 API 표면, 의사 콘솔 인프라, 터미널, 구성 속성 시트, 
    - 프로세스 로더 내의 메커니즘 및 스텁 및 이러한 형태의 애플리케이션의 작동과 관련된 모든 유틸리티는 
    - 이 그룹에 속한 것으로 간주된다.

- **Console Host**
    - Windows 콘솔 호스트 `conhost.exe`는 모든 Windows Console API에 대한 서버 애플리케이션뿐만아니라 
    - 명령줄 애플리케이션을 사용하기 위한 클래식 Windows 사용자 인터페이스입니다.

- **Terminal**
    - 터미널은 명령줄 애플리케이션에 대한 사용자 인터페이스 및 상호 작용 모듈이다. 
    - 현재는 디스플레이 모니터, 키보드 및 양방향 직렬 통신 채널이 있는 물리적 디바이스였던 것을 소프트웨어로 표현하고 있다.