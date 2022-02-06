### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 3
- #### 상속
    - 베이스 클래스와 파생 클래스의 메모리 레이아웃
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
            Cat(int age, const char* name);
        private:
            char* mName;
        };
        ```
        ```C++
        // Animal.cpp
        Animal::Animal(int age)
            : mAge(age)
        {
        }

        // Cat.cpp
        Cat::Cat(int age, const string& name)
            : Animal(age)
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
    - 생성자 호출 순서
    - 소멸자 호출 순서
    - 베이스 클래스와 매개변수 없는 생성자