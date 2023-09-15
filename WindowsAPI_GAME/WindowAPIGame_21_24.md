## Path
  
### PathMgr
경로를 관리하는 클래스
- Init()
- const wchar_t* GetContentPath()
- wchar_t content_path_[255]
  - solution 폴더까지 절대경로 값을 얻는다.
  - solution에서 resource가 있는 폴더까지 경로를 지정한다.
  - content_path_
    - solution 경로 + resource 경로

### 빌드한 결과물을 지정한 폴더로 지정하기
- 프로젝트를 빌드했을 때 만들어지는 실행파일을 어디에 출력할 것인가를 지정할 때
  - 속성 -> 일반 -> 출력 디렉토리
    - 디버그
      - `$(SolutionDir)Output\bin_debug\`
    - 릴리즈
      - `$(SolutionDir)Output\bin\`
- 실행 파일 이름 수정
  - 속성 -> 일반 -> 대상 이름
- 작업 디렉토리
  - 속성->디버깅->작업 디렉터리
    - `$(SolutionDir)Output\bin\`

## c-style
모든 문자 표현은 c-style로 작성한다.
```C++
// 수정 불가한 문자 : 데이타 섹션에 저장
const wchar_t* sz = L"\\bin\\content\\";

// 수정 가능한 문자열 : 스택에 저장
wchar_t dest[255] = {0, };
wchar_t source[] = L"\\bin\\content\\";
size_t source_length = wcsnlen_s(source, 255);

wcsncat_s(dest, 255, source , source_length);
```


## Resource

### ResourceMgr
texture, sound 등 자원을 전체적으로 관리한다.
리소스가 없으면 로딩해서 주고, 있으면 생성된 걸 지급한다.
- Texture* LoadTexture()
  - 비트맵 이미지 하나당 Texture class 하나 저장 그래서 중복 체크해서 없으면
  - Texture 생성한다.
  - 즉, 첫번째 플레이어가 최초 로딩을 시도할 것이고
  - 그 다음부터는 생성된 이후의 플레이어들은 생성된 것을 지급받는다.
- map<const wchar_t*, Texture*> textures_
  - 저장하고 관리한다. 
- Texture* FindTexture(const wchar_t* key)
  - 중복 체크 

### GameResource
- key
- relative_path

#### Texture
지정된 경로의 비트맵 이미지를 로딩해서 비트맵 핸들과 memoryDC를 생성하고 연결하는 역할을 한다.
- void Load(const wchar_t* file_path)
- Uint GetWidth()
- Uint GetHeight()
- HDC GEtMemoryDC()

#### Sound


## Player
메모리적인 측면에서 리소스를 사용하려는 모습은 플레이어, 몬스터 이런 생성자에서 직접 텍스처를 로딩하는게 아니라 리소스를 관리하는 담당 매니저를 통해서 리소스를 로딩해야한다.

- player에 Texture을 적용한다.
  - player 생성될 때 Texture 로딩한다.
  - Texture* 을 ResourceMgr->LoadTexture 함수로 받아서
  - Randering 한다.
    - BitBlt()


## BITMAP

### 비트맵을 다루는 두 가지 방법
- 1. 비트맵을 리소스에 등록하여 사용
  - 비트맵 이미지를 VC에 넣어서 사용
  - 그래서 실행파일과 같이 만들어지기 때문에 실행파일 용량이 커진다.
  - 단점이 크다.
- 2. LoadImage() 함수를 이용하여 파일 경로로부터 읽어내는 방법
  - 복잡하지만 많이 사용

### 비트맵을 읽어 출력하는 순서
- 1. 비트맵의 핸들을 얻는다.
  - LoadBitmap()
  - LoadImage()
- 2. 메모리 DC 생성
  - 이미지를 메모리에 적정하기 위해
  - CreateCompatibleDC()
- 3. 메모리 DC에 비트맵 적용(복사)
  - 만들어진 메모리 DC에는 아무것도 없다.
  - 그래서 핸들과 메모리 DC를 연결 시켜준다.
  - SelectObject()
- 4. 비트맵 출력
  - BitBle(bm.bmWidth, bm.bmHeight, SRCCOPY);
- 5. 메모리 DC와 비트맵 제거
  - DeleteDC()
  - DeleteObject()

### 비트맴 확대축소 출력 함수
- 1. StretchBlt()
  - 확대축소
- 2. TransparentBlt()
  - 확대축소
  - 특정색 제외하고 출력
  - msimg32.lib 라이브러리 추가
    - 설정
    - 링커
    - 입력
    - 추가 종속성->편집(msimg32.lib)
    - `#pragma comment(lib, "Msimg32.lib")`

### 비트맵 정보
- 1. BITMAP 구조체
  - int bmType
  - int bmWidth
  - int bmHeight
  - int bmWidthBytes
  - BYTE bmPlanes
  - BYTE bmBitsPixel
  - LPVOID bmBits
- 2. GetObject()
  - BITMAP에 대한 정보를 알 수 있다.
  - bm.bmWidth
  - bm.bmHeight



### Windows Api 그래픽 출력 정리
1. 그래픽 출력
  - GDI
  - DC
2. 스톡 오브젝트와 커스텀 오브젝트
  - 펜, 브러시
3. 도형 출력
4. SelectObject(), DeleteObject()
5. 메모리 DC 사용순서
6. 비트맵 다루기
  - CreateCompatibleDC()
  - DeleteDC()
7. BitBlt(),
8. StretchBlt(),
9. TransparentBlt()
  - wingdi.h(Windows.h 포함)
  - Msimg32.lib(구현부분)









