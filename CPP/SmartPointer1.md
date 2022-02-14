### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 스마트(Smart) 포인터 1
- #### 원시 포인터의 문제점
    ```c++
    int main()
    {
        Vector* myVector = new Vector(10.f, 30.f);

        // ...

        delete myVector;

        return 0;
    }
    ```
    - 더 이상 포인터가 필요하지 않을 때 메모리를 해제해야 한다.
        - 가리키는 주소의 데이타를 해제 시켜야 메모리 누수가 없어진다.
        - 강제로 **delete**를 직접 호출해서 해제시켜야 한다.
    - **스마트 포인터**를 쓰면 메모리 해제를 위한 **delete**를 직접 호출할 필요가 없다.
    - 그리고 가비지 컬렉션보다 빠르다!

- #### 유니크 포인터(unique_ptr)
    - 포인터(원시 포인터)를 단독으로 소유
    - 원시(naked) 포인터는 누구하고도 공유하지 않음
    - 따라서 **복사나 대입 불가**
        - 복사 생성자, 대입 연산자 = delete
            ```c++
            std::unique_ptr<Vector> myVector(new Vector(10.f, 30.f));

            std::unique_ptr<Vector> copiedVector1(myVector);    // 컴파일 에러 (복사)
            std::unique_ptr<Vector> copiedVector2 = myVector;   // 컴파일 에러 (대입)
            ```
    - unique_ptr가 범위(scope)를 벗어날 때, 원시 포인터는 지워짐(delete)

- #### 유니크 포인터는 다음의 세 경우에 적합하다.
    - ##### 클래스에서 생성자/소멸자
        ```c++
        // OLD
        
        // Player.h
        class Player
        {
        private:
            Vector* mLocation;
        };

        // Player.cpp
        Player::Player(std::string name)
            : mLocation(new Vector())
        {            
        }

        Player::~Player()
        {
            delete mLocation;
        }
        ```
        ```c++
        // NEW

        // Player.h
        class Player
        {
        private:
            std::unique_ptr<Vector> mLocation;
        };

        // Player.cpp
        Player::Player(std::string name)
            : mLocation(new Vector())
        {            
        }

        // 소멸자에 delete 키워드 없음!!!
        ```
    - ##### 지역 변수
        ```c++
        // OLD

        #include "Vector.h"

        int main()
        {
            Vector* vector = new Vector(10.f, 30.f);

            vectr->Print();
            
            delete vector;

            // ...
        }
        ```
        ```c++
        // NEW

        #include <memory>
        #include "Vector.h"

        int main()
        {
            std::unique_ptr<Vector> vector(new Vector(10.f, 30.f));

            vectr->Print();
        
            // ...
        }
        ```
    - ##### STL 벡터에 포인터 저장하기
        ```c++
        // OLD

        int main()
        {
            std::vector<Player*> plyers; 

            players.push_back(new Player("Lulu"));
            players.push_back(new Player("Coco"));

            // players 삭제
            for (int i = 0; i < players.size(); ++i)
            {
                delete players[i];
            }

            players.clear();
            // ...
        }
        ```
        ```c++
        // NEW

        int main()
        {
            std::vector<std::unique_ptr<Player>> playerList;

            playerList.push_back(std::unique_ptr<Player>(new Player("Lulu")));
            playerList.push_back(std::unique_ptr<Player>(new Player("Coco")));

            // playerList 삭제
            playerList.clear();
            // ...
        }
        ```

- #### 유니크 포인터 만들기 - std::make_unique()
    ```c++
    // non-array
    template< class T, class... Args >
    unique_ptr<T> make_unique(Args&&... args);   // &&은 r-value

    std::unique_ptr<Vector> vector = std::make_unique<Vector>(10.f, 30.f);

    // array
    template< class T >
    unique_ptr<T> make_unique(std::size_t size);

    std::unique_ptr<Vector[]> vectors = std::make_unique<Vector[]>(20);
    ```

- #### 유니크 포인터 재설정하기 - reset()
    - 포인터를 교체한다.
    - std::unique_ptr가 재설정될 때, 소유하고 있던 원시 포인터는 자동으로 소멸됨
        ```c++
        int main()
        {
            std::unique_ptr<Vector> vector = std::make_unique<Vector>(10.f, 30.f);
            
            vector.reset(new Vector(20.f, 40.f));  // 포인터를 교체 : (10.f, 30.f) -> (20.f, 40.f)
                                                   // 재설정될 때, 소유하고 있던 원시 포인터는 자동으로 소멸
            vector.reset();  // vector = nullptr; 
            // ...
        }
        ```

- #### 포인터 가져오기 - get()
    - 원시 포인터를 반환한다.
        ```c++
        // Vector.cpp
        void Vector::Add(const Vector* other)
        {
            mX += other->mX;
            mY += other->mY;
        }
        ```
        ```c++
        int main()
        {
            std::unique_ptr<Vector> vector = std::make_unique<Vector>(10.f, 30.f);
            std::unique_ptr<Vector> anotherVector = std::make_unique<Vector>(20.f, 40.f);

            vector->Add(anotherVector.get());
            vector->Print();

            return 0;
        }
        ```
- #### 포인터 소유권 박탈하기 - release()
    - 원시 포인터에 대한 소유권을 박탈하고 원시 포인터를 반환한다.

- #### 포인터 소유권 옮기기 - std::move()
    - std::unique_ptr는 소유한 원시 포인터를 아무하고도 공유하지 않음
    - 즉, 주소 복사를 하지 않는다는 뜻이다.
    - 대신, 소유권을 다른 std::unique_ptr로 옮길 수 있다.
    - 단, 예외: `const std::unique_ptr`
    - **std::move()**
        - 개체 A의 모든 멤버를 포기하고 그 소유권을 B에게 주는 방법이다.
        - 메모리 할당과 해제가 일어나지 않음
        - 간단하게, A에 있는 모든 포인터를 B에게 대입하고 A에는 nullptr를 넣는다고 생각하자
        - 이게 어떻게 도는지 알려면 r-value와 이동(move) 생성자를 배워야 된다.
            ```c++
            int main()
            {
                std::unique_ptr<Vector> vector = std::make_unique<Vector>(10.f, 30.f);
                // vector -> (10.f, 30.f)

                std::unique_ptr<Vector> anotherVector(std::move(vector));
                // anotherVector -> (10.f, 30.f)
                // vector -> nullptr
            }
            ```
            ```c++
            std::vector<std::unique_ptr<Player>> players;

            std::unique_ptr<Player> coco = std::make_unique<Player>("Coco");
            players.push_back(std::move(coco));   // coco -> nullptr

            std::unique_ptr<Player> lulu = std::make_unique<Player>("Lulu");
            players.push_back(std::move(lulu));   // lulu -> nullptr
            ```

- #### std::unique_ptr
    ```c++
    // 매우 단순화
    template<typename T>
    class unique_ptr<T> final
    {
    public:
        unique_ptr(T* ptr) : mPtr(ptr) {}
        ~unique_ptr() { delete mPtr; }
        T* get() { return mPtr; }
        unique_ptr(const unique_ptr&) = delete;
        unique_ptr& operator=(const unique_ptr&) = delete;
    priavet:
        T* mPtr = nullptr;
    }
    ```

- #### std::unique_ptr 베스트 프랙티스
    - 직접 메모리 관리하는 것 만큼 빠름
    - RAII(Resource Acquisition Is Initialization) 원칙에 잘 들어맞음
        - 자원 할당은 개체의 수명과 관련되어 있음
        - 생성자에서 new 그리고 소멸자에서 delet
        - std::unique_ptr 멤버 변수가 이걸 해 줌
    - 실수하기 어려움
    - 모든 곳에 쓰자!
