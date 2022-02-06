### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 3
- #### 상속
    - 베이스 클래스와 파생 클래스의 메모리 레이아웃
        - 부모 생성자가 호출되어 메모리 공간을 만들고
        - 자식 생성자가 그 다음 위치에 메모리 공간을 만든다.
        - 즉, 부모 자식간의 메모리는 연속되게 되어 있고 언제나 부모 메모리부터 들어간다.        
        - [-------Cat-------]
        - [Animal]
        - [ mAge ][mName]
        - 그래서 부모가 먼저 호출되고 그 다음 자식이 호출되는 이유이다.
        ```C++
        // Animal.h
        class Animal
        {
        public:
            Animal(int age);
            void Move();
        private:
            int mAge;
        };

        // Cat.h
        class Cat : public Animal
        {
        public:
            Cat(int age, const char* name);  // age는 Animal이 알아서 할 일, Cat은 nName만 신경 써주면 된다.
        private:
            char* mName;
        };
        ```
        ```C++
        // Animal.cpp
        Animal::Animal(int age)   // 4. Cat이 Animal 호출 
            : mAge(age)           // 5. 그리고 초기화 2가 들어가고
        {
        }

        // Cat.cpp
        Cat::Cat(int age, const string& name) // 2. 처음에 Cat 생성자가 들어온다 => new Cat(2, "Mew");
            : Animal(age)                     // 3. 그 다음 부모 생성자를 호출
        {
            size_t size = strlen(name) + 1;   // 6.
            mName = new char[size];
            strcpy(mName, name);
        }
        ```
        ```C++
        // main.cpp
        Cat* myCat = new Cat(2, "Mew");  // 1. 스택에 myCat 변수 공간이 만들어진다.
        ```
        - stack
            - myCat[4096] ->
        - heep
            - [2][1028] -> [m][e][w][\0]

    - 생성자 호출 순서
        - 베이스 클래스의 생성자가 먼저 호출 됨
        - 그 다음으로 파생 클래스의 생성자가 호출 됨
        - 부모 클래스의 특정 생성자를 호출 할 때는 초기화 리스트를 사용해야 한다.
            ```C++
            Cat::Cat(int legs, int age, const string& callingName)
                : Animal(legs, age)   // 명시적 호출
                , mCallingName(callingName)
            {                
            }
            ```
    - 소멸자 호출 순서
        - 생성자 호출 순서와 정반대
        - 파생 클래스 소멸자의 마지막에서 베이스 클래스의 소멸자가 `자동적으로 호출`된다.
            ```C++
            // Animal.cpp
            Animal::~Animal()   // 4. 부모 소멸자 호출
            {                
            }

            // Cat.cpp
            Cat::~Cat()     // 2. 자식 소멸자부터 호출해 지운다.
            {
                delete mName;
                // 3. ~Animal()을 자동적으로 호출
            }

            // main.cpp
            delete myCat;  // 1. 할당 해제
            ```
    - 베이스 클래스와 매개변수 없는 생성자
        ```C++
        // Animal.cpp
        // 암시적 매개변수 없는 생성자 Animal()을 호출하면 mAge는 0으로 초기화
         Animal::Animal()  // 매개변수 없는 생성자
            : mAge(0)      // mAge는 0으로 초기화 된다. 2가 입력되지 않는다.
        {
        }
        // Animal.cpp
        // 암시적 매개변수 없는 생성자 Animal()을 호출하면 컴파일 에러
        Animal::Animal(int age)  // 매개변수 있는 생성자
            : mAge(age)           
        {
        }

        // Cat.cpp
        Cat::Cat(int age, const string& name)
            // 명시적 생성자 호출이 아니라 암시적 Animal()을 호출을 하면   
            // 컴파일러가 암시적으로 호출할 수 있는 부모 생성자는 Animal()이것 뿐인데
            // 매개변수가 있는 Animail(int age)를 호출하려니 컴파일 에러가 난다.               
        {
            size_t size = strlen(name) + 1;  
            mName = new char[size];
            strcpy(mName, name);
        }
        ```
        ```C++
        // main.cpp
        Cat* myCat = new Cat(2, "Mew");  
        ```

- #### 다형성과 관련있는 `멤버 함수의 메모리`에 대해 알아보자.