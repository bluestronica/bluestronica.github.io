### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 새로운 키워드 (C++ 11~)
- #### atuo
    - 자료형을 추론
    - 실제 자료형은 컴파일하는 동안 결정된다.
    - 그래서, 반드시 auto 변수를 초기화 해야한다.
        - auto x;  `// error`
        - auto x = "blues";  `// ok, 여기서 x는 const char*가 된다.`
    - auto를 사용하여 포인터와 참조를 받을 수 있음
        - 포인터 받을 때는 auto*
        - 참조를 받을 때는 auto&
            - const 참조를 받을 때는 가독성을 위해 const auto&를 쓰자!
            - const auto& name = object.GetName();
    - auto는 함수가 반환하는 걸 저장하는데 때론 유용하다.
        - 함수 반환형이 변해도 auto는 그대로
    - auto는 타이핑을 좀 줄여주지만 **가독성을 떨어뜨린다.**
    - 개인적으로 auto보다 실제 자료형을 선호한다.
    - 단, auto가 유용하게 쓰이는 곳이 있다
        - auto와 반복자 **(매우 유용)**
            ```c++
            for (std::vector<int>::const_iterator it = v.begtin(); it != v.end(); ++it)
            {
                // 이하 코드 생략
            }            

            for (auto it = v.begtin(); it != v.end(); ++it)
            {
                // 이하 코드 생략
            }
            ```
        - auto로 템플릿 받기
            ```c++
            MyArray<int>* a = new MyArray<int>(10);

            auto* a = new MyArray<int>(10);
            ```

- #### static_assert
    - **assert**
        - **실행 중**에 가정(어서션, assertion)이 맞는 지 평가
        - 오직 디버그 빌드에서만 작동한다.
        - 실패한 assert를 보려면 **반드시 프로그램을 실행**해야 한다.
            - 모든 코드 경로가 실행되었다고 어떻게 장담할까??
        - 모든 곳에 assert를 쓰자!
        ```c++
        // Class.cpp
        const Student* Class::GetStudentInfo(const char* name)
        {
            assert(name != NULL);  // 조건이 fasle면 실행을 멈춤
            // 이하 코드 생략...
        }
        ```
    - **static_ssert**
        - **컴파일 중에** 어서션 평가
        - 컴파일러가 assert 조건이 참인지 아닌 앎
        - 실패하면 컴파일러는 컴파일 에러를 뱉음
        - 많은 경우에 유용함
        - 베스트 프랙티스
            - 최대한 assert보다 **static_assert를 사용!**
            - 컴파일 중에 계산할 수 없는 경우(실행 도중에 결정되는 데이터 값)면 assert 쓰자!
        - 예시 1
        ```c++
            // Class.h
            struct Student
            {
                char name[64];
                char id[10];
                int currentSemester;
            };

            class Class
            {
            public:
                // ...
                const Student* GetStudentInfo(const char* name);
            };
            ```
            ```c++
            // Class.cpp
            const Student* Class::GetStudentInfo(const char* name)
            {
                static_assert(sizeof(Student) == 74, "Student structure size missmatch");
                // 이하 코드 생략...
            }
            ```
        - 예시 2
            ```c++
            // Class.h
            class Class
            {
            public:
                const static int Version = 1;
                // ...
            };
            ```
            ```c++
            // Main.cpp

            static_assert(Class::Version > 1, "You need higher viersion than 1.");
            ```
        - 예시 3
            ```c++
            // Student.h
            class Studen
            {
            public:
                static const int MAX_SCORES = 10;
                int GetScore(int index);
                // ...
            private:
                int mScores[MAX_SCORES];
            };
            ```
            ```c++
            // Student.cpp
            int Student::GetScore(int index)
            {
                static_assert(
                    sizeof(mScores) / sizeof(mScores[0]) == MAX_SCORES,
                    "The size of scores vector is not 10");
                // 이하 코드 생략
            }
            ```

- #### default / delete
    - 프로그래머에게 명시적으로 나타내는 의미가 있다.
    - **default 키워드를 사용하면**
        - 컴파일러가 특정한 생성자, 연산자 및 소멸자를 만들어 낼 수 있다.
    - **delete 키워드를 사용하면**
        - 컴파일러가 자동으로 생성자를 만들어 주지 않는다.
    - default / delete 키워드를 명시적으로 사용함으로써 
    - 이제 컴파일러가 코드를 생성하는 암시적 방식에 기댈 필요가 없어졌다.
    - 어디에나 default / delete 키워드를 넣자!
        ```c++
        // Dog.h
        class Dog
        {
        public:
            Dog() = default;
            Dog(const Dog& other) = delete;
            // ...
        }
        ```
        ```c++
        // Main.cpp
        Dog myDog;
        Dog copiedMyDog(myDog);  // 에러
        // ...
        ```

- #### final / override
    - **final 키워드**
        - final 키워드로 클래스 상속 막기 1
            ```c++
            // Animal.h
            class Animal final
            {
            public:
                virtual void SetWeight(int weight);
                // ...
            }
            ```
            ```c++
            // Dog.h
            class Dog : public Aniaml   // 에러, 상속이 안된다.
            {
            public:
                virtual void SetWeight(int weight);
                // ...
            }
            ```
        - final 키워드로 클래스 상속 막기 2
            ```c++
            // Dog.h
            class Dog final : public Animal  // Animal은 final 클래스가 아님
            {
            public:
                virtual void SetWeight(int weight);
                // ...
            }
            ```
            ```c++
            // Corgi.h
            class Corgi : public Dog   // 에러, 상속이 안된다.
            {
            public:
                virtual void SetWeight(int weight);
                // ...
            }
            ```
        - final 키워드로 함수 속성 막기
            ```c++
            // Animal.h
            class Animal
            {
            public:
                virtual void SetWeight(int weight) final;
                // ...
            }
            ```
            ```c++
            // Dog.h
            class Dog : public Aniaml
            {
            public:
                virtual void SetWeight(int weight) override;  // 에러
                // ...
            }
            ```
    - **override 키워드**
        - 가상함수를 재정의를 해서 사용하는지 파생클래스의 가상함수를 사용하는지 구분해준다.
        - override 키워드 사용
            - 가상 함수를 재정한다는 의미이다.
        - override 키워드 사용 안함
            - 파생클래스의 가상 함수를 사용한다는 의미힌다.
            ```c++
            // Animal.h
            class Animal
            {
            public:
                virtual void SetWeight(float weight);
                void PrintAll();
                // ...
            }
            ```
            ```c++
            // Dog.h
            class Dog : public Aniaml
            {
            public:
                virtual void SetWeight(float weight) override;  // OK

                // 컴파일 에러
                virtual void SetWeight(int weight) override; 

                // 컴파일 에러: 가상 함수가 아님
                void PrintAll() override;

                // ...
            }
            ```

- #### offsetof 매크로
    - #define offsetof(type, member)  /*implementation-defined*/
    - 매크로의 일종
    - 특정 멤버가 본인을 포함한 자료 구조의 시작점에서부터 몇 바이트만큼 떨어져 있는지 알려줌
    - 직렬화(serialize)나 역직렬화(deserialize)를 할 때 꽤나 유용하다.
        - 직렬화란 어떤 데이타를 파일에 쓰는 것
            - 프로그램에 사용할 데이터를 어느 곳에 저장할 때
        - 역직렬화란 파일에 있는 데이터를 개체(object)로 읽어 오는 것
            - 저장되어 있는 것을 프로그램에 쓸려고 오브젝트로 바꿀 때
    - 멤버들의 상대적 위치(offset) 구하기
        ```c++
        // Main.cpp
        struct Student
        {
            const char* ID;
            const char* Name;
            int CurrentSemester;
        };

        int main()
        {
            std::cout << "ID offset: " << offsetof(Student, ID) << std::endl;
            std::cout << "Name offset: " << offsetof(Studen, Name) << std::endl;
            std::cout << "CurrentSemester offset: " << offsetof(Student, CurrentSemester) << std::endl;

            return 0;
        }
        ```
