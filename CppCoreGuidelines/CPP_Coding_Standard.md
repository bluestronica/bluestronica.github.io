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

4. 클래스 맴버 변수명은 끝에 `_` 를 붙인다.
  ```C++
  class Core
  {
  protected:
    int department_id;
  private:
    HWND	main_hwnd_;
    POINT	resolution_;
  }
  ```

5. 열거형은 앞에 `E`를 붙인다.
   ```C++
   enum class ESceneType
   {
     START,
     LEVEL1,
     LEVEL2,
     TOOL,
  
     END
   };
   ```

6. 인터페이스는 앞에 `I`를 붙인다.
   ```C++
   class ISomeInterface;
   ```

7. 클래스 맴버 변수에 접근할 때는 항상 setter와 getter를 사용한다.
```C++
  class Employee
  {
  public:
      const string& GetName() const;
      void SetName(const string& name);
  private:
      string name_;
  }
```

8. 구조체는 오직 public 멤버 변수만 가질 있다. 구조체의 멤버 변수명은 카멜 표기법을 따르며, 구조체 안에서는 함수는 사용하지 않는다.
```C++
  struct Vec
  {
    float x;
    float y;
  }
```

9. const
  - 원칙적으로 모든 곳에 const를 사용한다. 여기에는 지역 변수와 함수 매개 변수도 포함된다.
  - 개체를 수정하지 않는 멤버 함수에는 모두 `const`를 붙인다.
    ```C++
    int GetAge() const;
    ```
  - 값(value)  형식의 변수를 const로 반환하지 않는다.
  - 포인터나 참조(reference)를 반환할 경우에만 `const` 반환한다.
















