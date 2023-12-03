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
  - public 메서드는 파스칼 표기법
    ```C++
    public:
      void DoSomething();
    ```
 - 그 외 다른 메서드는 카멜 표기법
    ```C++
    private:
      void doSomething();
    ```

3. 지역 변수 그리고 함수의 매개 변수는 소문자
  ```C++
   void SomeMethod(const int some_parameter);
   {
       int some_number;
       int id;
   }
  ```

4. 클래스 맴버 변수명은 소문자로 표현하고 끝에 `_` 를 붙인다.
  ```C++
  class Core
  {
  protected:
    int department_id_;
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

9. 대부분의 경우 함수 오버로딩을 피한다.
```C++
// 틀린 방식:
const Anim* GetAnim(const int index) const;
const Anim* GetAnim(const char* name) const;


// 올바른 방식:
const Anim* GetAnimByIndex(const int index) const;
const Anim* GetAnimByName(const char* name) const;
```

10. const
  - 원칙적으로 모든 곳에 const를 사용한다. 여기에는 지역 변수와 함수 매개 변수도 포함된다.
  - 개체를 수정하지 않는 멤버 함수에는 모두 `const`를 붙인다.
    ```C++
    int GetAge() const;
    void SetName(const string& name);
    ```
  - 값(value)  형식의 변수를 const로 반환하지 않는다.
  - 포인터나 참조(reference)를 반환할 경우에만 `const` 반환한다.
    ```C++
      const string& GetName() const;
      const Anim* GetAnimByIndex(const int index) const;
    ```
  - `const 반환을 위한` 함수 오버로딩은 허용한다.
    ```C++
      Anim* GetAnimByIndex(const int index);
      const Anim* GetAnimByIndex(const int index) const;
    ```
  - `const_cast`를 직접적으로 사용하지 않는다. 대신 `const`인 개체를 수정 가능한 형태로 변환해서 반환하는 함수를 만든다.

11. 파일 이름은 대소문자까지 포함해서 반드시 클래스 이름과 일치해야 한다.
```C++
class PlayerAnimation;

PlayerAnimation.cpp
PlayerAnimation.h
```
12. 어떤 이유로든 매개변수로 `nullptr`가 넘어올 수 있는 경우가 아니라면, 포인터 대신 `참조자(&)`를 사용하는 것을 원칙으로 한다.(예외는 다음 항목을 참고)

13. (예외)매개 변수가 클래스 내부에 저장될 때 포인터를 사용한다.
```C++
void AddMesh(Mesh* const mesh)
{
  mesh_collection_.push_back(mesh);
}
```

14. (예외)매개 변수가 void 포인터여야 하는 경우는 포인터를 사용한다.
```C++
void Update(void* const something)
{
}
```

15. (예외)함수에서 매개변수를 통해 값을 반환할 때(out 매개변수)는 포인터를 사용하며, 매개변수 이름 앞에 out을 붙인다.
```C++
uint32_t width;
uint32_t height;
GetScreenDimension(&width, &height);

void GetScreenDimension(uint32_t* const outWidth, uint32_t* const  outHeight)
{
    // out 매개 변수는 반드시 nullptr이 아니어야 한다.
    Assert(outWidth);
    Assert(outHeight);
}
```

16. 클래스 안에서 멤버 변수와 메서드의 등장 순서는 다음을 따른다.
- friend 클래스들
- public 메서드들
- protected 메서드들
- private 메서드들
- protected 변수들
- private 변수들













