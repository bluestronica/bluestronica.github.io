# 기본 구조
```c
// Client.cpp : 애플리케이션에 대한 진입점을 정의합니다.
//

#include "framework.h"
#include "Client.h"

#define MAX_LOADSTRING 100

// 전역 변수:
HINSTANCE hInst;                                // 현재 인스턴스입니다.
HWND g_hWnd;
WCHAR szTitle[MAX_LOADSTRING];                  // 제목 표시줄 텍스트입니다.
WCHAR szWindowClass[MAX_LOADSTRING];            // 기본 창 클래스 이름입니다.

// 이 코드 모듈에 포함된 함수의 선언을 전달합니다:
ATOM                MyRegisterClass(HINSTANCE hInstance);
BOOL                InitInstance(HINSTANCE, int);
LRESULT CALLBACK    WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    About(HWND, UINT, WPARAM, LPARAM);


// SAL 주석 언어 : _In_ , _In_opt_ 변수 용도를 나타내는 키워드
int APIENTRY wWinMain(_In_ HINSTANCE hInstance,  /* 실행 된 프로세스의 메모리 시작 주소 */
                     _In_opt_ HINSTANCE hPrevInstance, // 가상 메모리 이전의 변수, 지금은자기만의 가상 메모리를 가진다.
                     _In_ LPWSTR    lpCmdLine,  // 
                     _In_ int       nCmdShow)
{
    UNREFERENCED_PARAMETER(hPrevInstance);
    UNREFERENCED_PARAMETER(lpCmdLine);

    // TODO: 여기에 코드를 입력합니다.

    // 전역 문자열을 초기화합니다.
    LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
    LoadStringW(hInstance, IDC_CLIENT, szWindowClass, MAX_LOADSTRING);

    // 윈도우 정보 등록
    MyRegisterClass(hInstance);

    // 윈도우 생성 : 애플리케이션 초기화를 수행합니다:
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    // 단축키 테이블 정보 로딩
    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_CLIENT));

    MSG msg;

    // GetMessage
    // 메세지큐에서 메세지 확인 될 때까지 대기, 메세지가 없어도 함수는 종료하지 않는다.
    // GetMessage가 FALSE를 반환하면 프로그램이 종료된다.
    // msg.message == WM_QUIT 이면 FALSE 반환 -> 프로그램 종료
    // WM_QUIT이 먼저 발생하기 전에, 윈도우들 부터 다 종료되고 해제되는 메세지가 먼저 발생 하고 완료되면 
    // GetMessage는 FALSE를 반환해서 while 문이 종료되도록 짜여져있다.

    // PeekMessage
    // 메세지 유무와 상관없이 계속 반환
    // 메세지큐에서 메세지를 확인한 경우 true, 메세지큐에 메세지가 없는 경우 false를 반환한다.
    // 컴퓨터 관점에서 보면 메세지 없을 때가 거의 90%에 가깝기에 메세지가 없는 동안 
    // 컴퓨터를 돌리려고 하는 것이다.


    DWORD dwPrevCount = GetTickCount();
    DWORD dwAccCount = 0;

    while (true)
    {                 
        // 반환 된 메세지가 무(없으면) fasle, 유(있으면) true
        if (PeekMessage(&msg, nullptr, 0, 0, PM_REMOVE))
        {            
            int iTime = GetTickCount();

            if (WM_QUIT == msg.message)
            {
                break;
            }

            if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
            {
                TranslateMessage(&msg);
                DispatchMessage(&msg);
            }

            // 메세지 처리 시간을 계속 누적
            int iAdd = (GetTickCount() - iTime);
            dwAccCount += iAdd;
        }
        else
        {
            // 메세지가 없는 동안 호출

            DWORD dwCurCount = GetTickCount();
            if (dwCurCount - dwPrevCount > 1000)
            {
                float fRatio = (float)dwAccCount / 1000.f;

                // 문자열
                wchar_t szBuff[50] = {};
                swprintf_s(szBuff, L"비율 : %f", fRatio);
                //wsprintf(szBuff, L"비율 : %f", fRatio);

                // 윈도우창 제목에 문자열 입력
                SetWindowText(g_hWnd, szBuff);     

                dwPrevCount = dwCurCount;
                dwAccCount = 0;
            }
        }
    }

    return (int) msg.wParam;
}



//
//  함수: MyRegisterClass()
//
//  용도: 창 클래스를 등록합니다.
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_CLIENT));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_CLIENT);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}

//
//   함수: InitInstance(HINSTANCE, int)
//
//   용도: 인스턴스 핸들을 저장하고 주 창을 만듭니다.
//
//   주석:
//
//        이 함수를 통해 인스턴스 핸들을 전역 변수에 저장하고
//        주 프로그램 창을 만든 다음 표시합니다.
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // 인스턴스 핸들을 전역 변수에 저장합니다.

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);
   g_hWnd = hWnd;
   if (!hWnd)
   {
      return FALSE;
   }

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}

//
//  함수: WndProc(HWND, UINT, WPARAM, LPARAM)
//
//  용도: 주 창의 메시지를 처리합니다.
//
//  WM_COMMAND  - 애플리케이션 메뉴를 처리합니다.
//  WM_PAINT    - 주 창을 그립니다.
//  WM_DESTROY  - 종료 메시지를 게시하고 반환합니다.
//
//

#include <vector>

struct g_tObjInfo
{
    POINT ptObjPos;
    POINT ptObjScal;
};

std::vector<g_tObjInfo> g_ObjInfo;

POINT g_ptLT;
POINT g_ptRB;

bool g_blbDown = false;


POINT g_ptObjPos = { 500, 300 };  // 그림의 중심
POINT g_ptObjScal = { 100, 100 }; // 크기

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
    case WM_COMMAND:
        {
            int wmId = LOWORD(wParam);
            // 메뉴 선택을 구문 분석합니다:
            switch (wmId)
            {
            case IDM_ABOUT:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
                break;
            case IDM_EXIT:
                DestroyWindow(hWnd);
                break;
            default:
                return DefWindowProc(hWnd, message, wParam, lParam);
            }
        }
        break;
    case WM_PAINT:  // 무효화 영역(Invalidate)이 발생한 경우
        {
            PAINTSTRUCT ps;

            // Device Context 만들어서 ID를 반환
            HDC hdc = BeginPaint(hWnd, &ps); // hdc는 지금 hWnd를 목적지로 해서 그림을 그리겠다는 의미
            // DC의 목적지는 hWnd
            // DC의 펜은 기본펜(Black)
            // DC의 브러쉬는 기본 브러쉬(White)

            // 1. 직접 펜을 만든다. 빨간색 실선 1pt
            HPEN hRedPen = CreatePen(PS_SOLID, 1, RGB(255, 0, 0));
            HBRUSH hBlueBrush = CreateSolidBrush(RGB(0, 0, 255));

            // 2. 직접 펜을 만들어서 DC에 선택(적용)하도록 지급
            // 2-1 기존 DC에 있던 디폴트 펜을 받아온다. 기본 펜 ID 값을 받아 둔다.
            HPEN hDefaultPen = (HPEN)SelectObject(hdc, hRedPen);
            HBRUSH hDefaultBrush = (HBRUSH)SelectObject(hdc, hBlueBrush);

            // 3. 변경된 펜으로 사각형 그림
            if (g_blbDown == true)
            {
                Rectangle(hdc, g_ptLT.x, g_ptLT.y, g_ptRB.x, g_ptRB.y);
            }
            // BeginPaint에서 반환한 hdc를 이용해서 목적지 hWnd(윈도우)에 사각형을 그리겠다는 의미

            for (size_t i = 0; i < g_ObjInfo.size(); i++)
            {
                // 중심값과 scal
                Rectangle(hdc,
                    g_ObjInfo[i].ptObjPos.x - g_ObjInfo[i].ptObjScal.x / 2,
                    g_ObjInfo[i].ptObjPos.y - g_ObjInfo[i].ptObjScal.y / 2,
                    g_ObjInfo[i].ptObjPos.x + g_ObjInfo[i].ptObjScal.x / 2,
                    g_ObjInfo[i].ptObjPos.y + g_ObjInfo[i].ptObjScal.y / 2);
            }

            // 4. hdc에 디폴트 펜으로 다시 교체
            // DC의 펜을 원래 펜으로 되돌림
            SelectObject(hdc, hDefaultPen);
            SelectObject(hdc, hDefaultBrush);

            // 5. 사용하지 않는 직접 만든 펜을 삭제
            // 다 쓴 Red펜 삭제 요청
            DeleteObject(hRedPen);
            DeleteObject(hBlueBrush);
            
            // 윈도우 핸들
            // 윈도우 좌표, 픽셀 : 하나의 픽셀은 3바이트(RGB) 0~250(R),0~250(G), 0~250(B)
            // HDC ?
            // dc : 그리기 작업 수행, 필요한 data 집합
            // 윈도우에 무언가를 그릴려면 항상 DC를 이용해서 그려야 한다.
            // DC를 가져와서 그림을 그린다는 것이 어떤 의미이냐면
            // 해당 DC가 목적지로 하고 있는 그림판에 
            // 그 DC 안에 들어 있는 PEN 정보로 또는 그 DC 안에 들어 있는 BRUSH 정보를
            // 이용해서 그림을 그리겠다는 뜻이 된다. DC가 그 종합적인 정보를 관리하고 있다.

            

            // 그리기 종료
            EndPaint(hWnd, &ps);
        }
        break;
    case WM_KEYDOWN:  // 포커싱 된 상태에서 키가 눌러지면 메세지가 발생
    {
        switch (wParam)
        {
        case VK_UP:
            g_ptObjPos.y -= 10;
            InvalidateRect(hWnd, nullptr, true);   // 무효화 영역을 함수로 직접 설정
            break;
        case VK_DOWN:
            g_ptObjPos.y += 10;
            InvalidateRect(hWnd, nullptr, true);   // 무효화 영역을 함수로 직접 설정
            break;

        case VK_LEFT:
            g_ptObjPos.x -= 10;
            InvalidateRect(hWnd, nullptr, true);   // 무효화 영역을 함수로 직접 설정
            break;

        case VK_RIGHT:
            g_ptObjPos.x += 10;
            InvalidateRect(hWnd, nullptr, true);   // 무효화 영역을 함수로 직접 설정
            break;
        }
    }
        break;
    case WM_LBUTTONDOWN:
    {
        g_ptLT.x = LOWORD(lParam);
        g_ptLT.y = HIWORD(lParam);

        g_blbDown = true;
    }
        break;
    case WM_MOUSEMOVE:
    {
        g_ptRB.x = LOWORD(lParam);
        g_ptRB.y = HIWORD(lParam);
        InvalidateRect(hWnd, nullptr, true);
    }        
        break;
    case WM_LBUTTONUP:
    {
        // 업 하는 순간 사각형 정보 등록
        g_tObjInfo objInfo = {};

        objInfo.ptObjPos.x = (g_ptLT.x + g_ptRB.x) / 2;
        objInfo.ptObjPos.y = (g_ptLT.y + g_ptRB.y) / 2;

        objInfo.ptObjScal.x = abs(g_ptLT.x - g_ptRB.x);
        objInfo.ptObjScal.y = abs(g_ptLT.y - g_ptRB.y);

        g_ObjInfo.push_back(objInfo);

        g_blbDown = false;
        InvalidateRect(hWnd, nullptr, true);
    }
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// 정보 대화 상자의 메시지 처리기입니다.
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}

```
