### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 가변 인자(Variadic) 템플릿
- #### 가변 인자 템플릿
    - 다양한 매개변수 개수와 자료형을 지원하는 클래스 또는 함수 템플릿
    - 매개변수 목록에서 **생략 부호(...)**를 씀

- #### 가변 인자 템플릿 함수
    ```c++
    template <typename... Arguments>
    <반환형> <함수명>(Arguments... args)
    {        
    }
    ```
    - std::make_unique()
        ```c++
        template<typename T, typename... Args>
        std::unique_ptr<T> make_unique(Args&&... args)
        {
            return std::unique_ptr<T>(new T(std::forward<Args>(args)...));
        }
        ```
        ```c++
        // Cat::CAt(std::string name, int age);
        std::unique_ptr<Cat> myCat = std::make_unique<Cat>("Lulu", 2);

        // Vec2::Vec2(float x, float y);
        std::unique_ptr<Vec2> uiLoc = std::make_unique<Vec2>(10.f, 15.f);

        // Vec3::Vec3(float x, float y, float z);
        std::unique_ptr<Vec3> playerLoc = std::make_unique<Vec3>(20.f, 1.f, 0.f);
        ```

- #### 가변 인자 템플릿 활용
    - 활용법을 생각하기가 정말로 어려움
    - 주로 인자 전달용
        - std::make_unique();
    - 몇몇 다른 아이디어들도 있음, 허나 대개 실요적이라는 생각이 안 듦
        - 구글 검색 키워드: "practical uses for variadic templates"
    - 생성할 때 인자의 개수가 달라질 때...
        