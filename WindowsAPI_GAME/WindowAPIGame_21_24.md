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
texture, sound 등 자원을 관리한다.
- Texture* LoadTexture()
  - 비트맵 이미지 하나당 Texture class 하나 저장 그래서 중복 체크해서 없으면
  - Texture 생성한다. 
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


### Player
player에 Texture을 적용한다.
- Texture* 을 ResourceMgr->LoadTexture 함수로 받아서
- Randering 한다.
  - BitBlt()


## BITMAP

### 비트맵을 다루는 두 가지 방법

1. 비트맵을 리소스에 등록하여 사용
  a.비트맵 이미지를 VC에 넣어서 사용
  b.그래서 실행파일과 같이 만들어지기 때문에
  c.실행파일 용량이 커진다.
  d.단점이 크다.

2. LoadImage() 함수를 이용하여 파일 경로로부터 읽어내는 방법
  - 복잡하지만 많이 사용


### 비트맵을 읽어 출력하는 순서
1. 비트맵의 핸들을 얻는다.
  a. LoadBitmap()
  b. LoadImage()

2. 메모리 DC 생성
  a. 이미지를 메모리에 적정하기 위해
  b. CreateCompatibleDC()

3. 메모리 DC에 비트맵 적용(복사)
  a. 만들어진 메모리 DC에는 아무것도 없다.
  b. 그래서 핸들과 메모리 DC를 연결 시켜준다.
  c. SelectObject()

4. 비트맵 출력
  a.BitBle(bm.bmWidth, bm.bmHeight, SRCCOPY);

5. 메모리 DC와 비트맵 제거
  a. DeleteDC()
  b. DeleteObject()


### 비트맴 확대축소 출력 함수
1. StretchBlt()
  a. 확대축소
2. TransparentBlt()
  a. 확대축소
  b. 특정색 제외하고 출력
  c. msimg32.lib 라이브러리 추가
    i. 설정
    ii. 링커
    iii. 입력
    추가 종속성->편집(msimg32.lib)

### 비트맵 정보
1. BITMAP 구조체
  a. int bmType
  b. int bmWidth
  c. int bmHeight
  d. int bmWidthBytes
  e. BYTE bmPlanes
  f. BYTE bmBitsPixel
  g. LPVOID bmBits
2. GetObject()
  a. BITMAP에 대한 정보를 알 수 있다.
  b. bm.bmWidth
  c. bm.bmHeight













