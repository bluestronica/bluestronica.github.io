### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 스마트(Smart) 포인터 3
- #### 공유 포인터(shared_ptr)
    - 두 개의 포인터를 소유
        - 데이터 ptr : 데이터(원시 포인터)를 가리키는 포인터
        - 제어 블록 ptr : 제어 블록을 가리키는 포인터
    - std::unique_ptr와 달리, 포인터를 다른 std::shared_ptr와 공유할 수 있음
    - 참조 카운팅 기반
    - 원시 포인터는 어떠한 std::shared_ptr에게 참조되지 않을 때 소멸됨
    - **공유 포인터 만들기**
        ```c++
        int main()
        {
            std::shared_ptr<Vector> vector = std::make_shared<Vector>(10.f, 30.f);
            // ...
        }            
        ```
    - **포인터의 소유권을 공유하기**
        ```c++
        int main()
        {
            
            std::shared_ptr<Vector> vector = std::make_shared<Vector>(10.f, 30.f);
            std::shared_ptr<Vector> copiedVector = vector;   // vector는 공유 포인터
            // ...
        }            
        ```
    - **포인터 재설정하기**
        - 원시 포인터를 해제한다.
        - 참조 카운트가 1 줄어듦
        - nullptr를 대입하는 것과 같음
            ```c++
            int main()
            {
                
                std::shared_ptr<Vector> vector = std::make_shared<Vector>(10.f, 30.f);
                std::shared_ptr<Vector> copiedVector = vector;   // vector는 공유 포인터
                vector.reset();   // vector = nullptr;과 같음
                // ...
            }            
            ```
    - **참조 횟수 구하기**
        - 원시 포인터를 참조하고 있는 std::shared_ptr의 개수를 반환한다.
            ```c++
            int main()
            {
                std::shared_ptr<Vector> vector = std::make_shared<Vector>(10.f, 30.f);
                std::cout << "vector: " << vector.use_count() << std::endl;

                std::shared_ptr<Vector> copiedVector = vector;
                std::cout << "vector: " << vector.use_count() << std::endl;
                std::cout << "copiedVector: " << copiedVector.use_count() << std::endl;

                return 0;
            }
            ```

- #### 약한 포인터(weak_ptr)
    - 약한 참조는 원시 포인터 해제에 영향을 끼치지 않음
    - 약한 참조 카운트는 약한 참조의 수를 저장하는 데 사용됨
    - 약한 참조로 참조되는 개체는 강한 참조 카운트가 0이 될 때 소멸됨
    - 순환 참조 문제의 해결책!!
    - **약한 포인터 만들기**
        ```c++
        int main()
        {
            std::shared_ptr<Person> owner = std::make_shared<Person>("Pope");
            std::weak_ptr<Person> weakOwner = owner;

            return 0;
        }
        ```
    - **약한 포인터로 공유 포인터 만들기**
        - 원시 포인터가 존재하면 공유 포인터를 만든다.
            ```c++
            int main()
            {
                std::shared_ptr<Person> owner = std::make_shared<Person>("Lulu");
                std::weak_ptr<Person> weakOwner = owner;

                std::shared_ptr<Person> lockedOwner = weakOwner.lock();

                return 0;
            }
            ```
    - **공유 포인터가 존재하는지 확인하기**
        ```c++
        int main()
        {
            std::shared_ptr<Person> owner = std::make_shared<Person>("Lulu");
            std::weak_ptr<Person> weakOwner = owner;

            std::shared_ptr<Person> lockedOwner = weakOwner.lock();
            if (ptr == nullptr)
            {
                // ...
                // ptr이 nullptr이면 
                // weakOwner는 weak 참조는 있는데 strong 참조는 없다는 얘기다.
                // 개체를 소멸시켰다는 것이다.
            }

            return 0;
        }
        ```
    - **순환 참조를 해결해 보자**
        ```c++
        // Person.h
        #inlcude <iostream>
        #include <string>
        #include "Pet.h"

        class Pet;

        class Person
        {
        public:
            void SetPet(const std::shared_ptr<Pet>& pet);
            Person(const std::string& name);
            Person() = delete;
            ~Person();
        private:
            std::string mName;
            std::shared_ptr<Pet> mPet;
        };
        ```
        ```c++
        // Pet.h
        #include <iostream>
        #include <string>
        #include "Person.h"

        class Person;

        class Pet
        {
        public:
            void SetOwner(const std::shared_ptr<Person>& owner);
            Pet(const std::string& name);
            Pet() = delete;
            ~Pet();
        private:
            std::string mName;
            std::shared_ptr<Person> mOwner;
        }
        ```
        ```c++
        // Person.h
        #include <iostream>
        #include <string>
        #include "Pet.h"

        class Pet;

        class Person
        {
        public:
            void SetPet(const std::shared_ptr<Pet>& pet);
            Person(const std::string& name);
            Person() = delete;
            ~Person();
        private:
            std::string mName;
            std::shared_ptr<Pet> mPet;
        }
        ```
        ```c++
        // Pet.h
        #include <iostream>
        #include <string>
        #include "Person.h"

        class Person;

        class Pet
        {
        public:
            void SetOwner(const std::shared_ptr<Person>& owner);
            Pet(const std::string& name);
            Pet() = delete;
            ~Pet();
        private:
            std::string mName;
            std::weak_ptr<Person> mOwner;  // 약한참조
        }
        ```

- #### 정리
    - ##### std::sahred_ptr
        - 강한 참조
        - 강한 참조 카운트를 늘림
        - 직접적으로 사용할 수 있음
        - 원시 포인터가 확실히 존재하기 때문
    - ##### std::weak_ptr
        - 약한 참조
        - 약한 참조 카운트를 늘림
        - 직접적으로 사용할 수 없음
        - lock을 써서 std::shared_ptr가 여전히 존재하는지 확인해야 함

