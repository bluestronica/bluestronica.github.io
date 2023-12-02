## C++ 코딩 표준

### 기본원칙
1. 가독성을 최우선으로 한다.
2. 문제가 있을 경우 최대한 빨리 크래시가 나거나 assert에 걸리도록 코드를 작성한다.
3. 기본적으로 통합개발환경(IDE)의 자동 서식을 따른다.

### 메인 코딩 표준
1. 클래스, 구조체는 파스칼 표기법
  ```C++
  class PlayerManager;
  struct AnimationInfo;
  ```
2. 메서드는 동사-목적어 쌍으로 표기 
  - a. public 메서드는 파스칼 표기법
    ```C++
    public:
      void DoSomething();
    ```
 - b. 그 외 다른 메서드는 카멜 표기법
    ```C++
    private:
      void doSomething();
    ```
3. 지역 변수 그리고 함수의 매개 변수는  카멜 표기법
  ```C++
   void SomeMethod(const int someParameter);
   {
       int someNumber;
       int id;
   }
  ```
4. 클래스 맴버 변수명은 끝에 **_**를 붙인다.
  ```C++
  private:
  	void createBrushPen();
  
  	HWND	main_hwnd_;
    POINT	resolution_;
  ```
