### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 3
- #### 상속
    - 베이스 클래스와 파생 클래스의 메모리 레이아웃
        - 부모 생성자가 호출되어 메모리 공간을 만들고
        - 자식 생성자가 그 다음 위치에 메모리 공간을 만든다.
        - 즉, 부모 자식간의 메모리는 연속되게 되어 있고 언제나 부모 메모리부터 들어간다.        
        - [-------Cat-------]
        - [Animal]
        - [-mAge-][-mName-]
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
    - 베이스 클래스와 매개변수 없는 생성자