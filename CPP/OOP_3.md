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
    - 멤버 함수도 메모리 어딘가에 위치해 있음
    - 그런데 `각 개체마다` 멤버 함수의 메모리가 잡혀 있을까?
    - `멤버 함수`는 컴파일 시에 `딱 한 번만 메모리에 할당`된다.
        - 저수준에서는 멤버 함수는 전역 함수와 그다지 다르지 않다.
        ```C++
        // main.cpp
        myCat->GetName();
            mov     ecx, dword  ptr[myCat]   // myCat 주소를 저장
            call    Cat::GetName(0A16C7h)

        yourCat->GetName();
            mov     ecx, dword ptr[yourCat]  // yourCat 주소를 저장
            call    Cat::GetName(0A16C7h)

        
        // myCat 포인터를 ecx에 저장한다.
        // GetName(0A16C7h)은 코드섹션에 저장된 주소 0A16C7h에 위치한 Cat클래스의 GetName()를 가리킨다.
        // 그 다음 ecx에 저장된 myCat의 주소를 꺼내서 사용한다.
        // return ptr->mName;   
        // myCat의 주소값이 ptr로 사용될 것이다. 그래서 myCat의 mName을 반환한다.

        // yourCat도 똑같이 코드섹션에 저장된 주소 0A16C7h에 위치한 Cat클래스의 GetName()를 가리킨다.
        // yourCat의 mName이 반환될 것이다.
        ```
    - 그래서 상속을 통한 `함수 오버라이딩`이 가능해진다.

- #### 정적 바인딩 - 멤버 함수
    - C++의 기본 동작은 정적 바인딩이다.
    - 생성된 클래스가 아니라 `타입의 각 멤버 함수`가 실행한다.
        ```C++
        class Animal
        {
        public:
            void Speak();  // "An animal is speaking"을 출력
        }

        class Cat : public Animal
        {
        public:
            void speak();  // "Meow"을 출력
        }
        ```
        ```C++
        // main.cpp
        Cat* myCat = new Cat();
        myCat->Speak();  // Meow

        Animal* yourCat = new Cat();
        yourCat->Speak();  // An animal is speaking
        ```

- #### 동적 바인딩 - 다형성의 핵심인 가상(virtual) 멤버 함수
    - 생성된 개체의 멤버 함수가 언제나 호출된다.
    - 동적 바인딩/늦은 바인딩
        - 실행 중에 어떤 함수를 호출할지를 결정한다.
        - 당연히 정적 바인딩보다 느림
    - 이를 위해 가상 테이블이 생성됨
        - 모든 가상 멤버 함수의 주소를 포함
        - 물론 클래스 마다 하나
        - 실행 중에 어떤 타입이고 어떤 함수 호출할지 그 과정을 처리하는게 가상 테이블이 하는 것이다.
    - 힙에 개체를 생성할 때, 해당 클래스의 가상 테이블 주소가 함께 저장 된다.
    - Cat클래스(코드섹션) <- Cat가상테이블 <- MyCat개체(힙)
    - 컴파일 시
        - 코드 섹션
            - Animal 클래스
                - void Move(Animal* ptr) const {}    
                - void Speak(Animal* ptr) const {}   
            - Cat 클래스
                - void Move(Cat* ptr) const {}  
                - void Speak(Cat* ptr) const {}  <-    
        - 가상 테이블
            - Animal 가상 테이블(Ox009ED0)
                - Animal::Move()의 주소    
                - Animal::Speak()의 주소   
            - Cat 가상 테이블(Ox009ED8)
                - Cat::Move()의 주소  
                - Cat::Speak()의 주소  <-
    - 실행 중
        - heap
            - MyCat
                - mAge 
                - mName
                - 가상 테이블 주소(Ox009ED8)  ->
                    - 개체가 생성되면 개체의 가상 테이블 주소를 가진다.
                    - Cat 가상 테이블이 Ox009ED8 있으니
                    - Speak()는 두 번째 함수니깐 Ox009EDC에 가서
                    - 그기에 저장된 주소가 가리키는 곳에 있겠다는 것을 알 수 있다.
            - YourCat
                - mAge
                - mName
                - 가상 테이블 주소(Ox009ED8)  ->
        ```C++
        class Animal
        {
        public:
            virtual void Move();   // 가상함수 선언, 언제나 생성된 개체의 멤버 함수가 호출된다.
            virtual void Speak();  // 가상함수 선언, An animal is speaking
        };

        class Cat : Animal
        {
        public:
            void Move();
            void Speak();  // Meow
        }
        ```
        ```C++
        // main.cpp
        Cat* myCat = new Cat();
        myCat->Speak();  // Meow , Speak()는 virtual 멤버 함수이기 때문에 힙에 생성된 개체 Cat()의 멤버함수가 호출된다.

        Animal* yourCat = new Cat();
        yourCat->Speak(); // Meow , Speak()는 virtual 멤버 함수이기 때문에 힙에 생성된 개체 Cat()의 멤버함수가 호출된다.
        ```
- #### C++ 다형성의 이해
    - C++의 기본적인 행동은 `선언한 타입의 멤버 함수`에 접근한다.
    - 단, virtual 키워드로 멤버 함수를 선언함으로써 타입이 아닌 
    - 힙에서 생성한 `개체의 멤버 함수`에 접근한다.
    - 게임에서 부모 몬스터를 상속받은 다양한 종류의 자식 몬스터가 있다.
    - 이들 모들 모두 동시에 move() 움직이게 하고 싶을 때
    - 부모 몬스터* 타입의 배열을 만들어 자식 몬스터들을 집어 넣는다.
    - 그래서 for문을 돌려서 자식 몬스터.move()를 실행한다.
    - 물론 move() 멤버함수는 virtual함수가 되어야한다.
    - 왜냐하면 자식 몬스터들 마다 각각 다른 형태의 움직임으로 걷고 이동해야 하기때문이다.
    - 그래서 자식 몬스터.move() 멤버함수를 호출해야한다.

- #### 가상(virtual) 소멸자
    - 모든 클래스마다 가상 소멸자 할 것!!
        - C++ 14/17에 해결책 있음
    - 가상 함수가 필요 없을 때
        - 클래스 만들었느데 상속을 받지 않았을 때
        - 상속을 받았지만 새로운 메모리 할당을 받지 않았을 때
    ```C++
    // Animal.h
    class Animal
    {
    public:
        virtual ~Animal();    // 가상 소멸자 선언
    private:
        int mAge;
    };

    //Cat.h
    class Cat : public Animal
    {
    public:
        // 여기는 virtual 키워드 생략 가능
        virtual ~Cat();
    private:
        char* mName;
    };
    ```
    ```C++
    // Cat.cpp
    Cat::~Cat()
    {
        delete mName;
    }
    ```

- #### 순수 가상함수
    - 구현체가 없는 멤버 함수
    - 파생 클래스가 구현해야 한다.
    ```C++
    class Animal
    {
    public:
        virtual void Speak() = 0;      // 순수 가상함수
        virtual float GetArea() = 0;   // 순수 가상함수
    private:
        int mAge;
    };
    ```

- #### 추상 클래스
    - 순수 가상함수를 가지고 있는 베이스 클래스를 추상 클래스라 한다.
        - 추상 클래스에서 개체를 만들 수 없음
        - 추상 클래스를 포인터나 참조형으로는 사용 가능
    ```C++
    class Animal
    {
    public:
        virtual void Speak() = 0;
    private:
        int mAge;
    };
    ```
    ```C++
    // 추상 클래스를 개체로 만들 수 없음
    Animal myAnimal;                  // 불가능
    Animal* myAnimal = new Animal();  // 불가능

    // 추상 클래스를 포인터나 참조의 type으로는 사용 가능
    Animal* myCat = new Cat();    // OK
    Animal& myCatRef = *myCat;    // OK
    ```

- #### 인터페이스
    - C++은 자체적으로 인터페이스를 지원하지 않은다.
    - 순수 추상 클래스를 사용하여 java의 인터페이스 흉내
        - 순수 가상 함수만 가짐
        - 멤버 변수는 없음
        ```C++
        // IFlyable.h
        class IFlyable
        {
        public:
            virtual void Fly() = 0;
        };

        // IWalkable.h
        class IWalkable
        {
        public:
            virtual void Walk() = 0;
        };
        ```
        ```C++
        // Bat.h
        class Bat : public IFlyable, public IWalkable
        {
        public:
            void Fly();
            void Walk();
        };

        // Cat.h
        class Cat : public IWalkable
        {
        public:
            void Walk();
        };
        ```
        ```c++
        // Bat.cpp
        void Bat::Fly() const
        {
            std::cout << "A bat is flying" << std::endl;
        }

        void Bat::Walk() const
        {
            std::cout << "A bat is walking" << std::endl;
        }

        // Cat.cpp
        void Cat::Walk() const
        {
            std::cout << "A cat is walking" << std::endl;
        }
        ```
        ```c++
        // Main.cpp
        Bat bat;
		Pig pig;

		bat.Fly();
		bat.Walk();

		// pig.Fly();
		pig.Walk();
        ```



### 정리
```C++
상속
	- 베이스 및 파생 클래스의 메모리 레이아웃
	- 생성자 호출순서
	- 소멸자 호출순서
	- 베이스 클래스와 매개변수 없는 생성자!


다형성
	- 멤버함수의 메모리
		○ 개체의 각 멤버함수는 컴파일 시에 딱 한 번만 메모리에 할당 된다.
		○ 저수준에서는 멤버함수는 전역함수와 그다지 다르지 않다.


비 가상 함수 (정적 바인딩)
	- 정적 바인딩
		○ void Speak();
		○ 생성된 클래스의 멤버함수가 실행되는 것이 아니라 Cat*, Animal* 타입의 멤버함수가 실행된다.
		○ C++ 에서는 정적 바인딩을 하기 때문이다.


가상(virtual) 함수 (다형성의 핵심) (동적 바인딩)
	- virtual void Speak();
		○ Animal* 타입의 멤버함수가 아니라
		○ Cat* 타입의 멤버함수 Speak() 실행 된다.
	- 그래서 언제나 파생 클래스의 멤버함수가 호출된다.
		○ 부모의 포인터 또는 참조를 사용 중이더라도
		○ (핵심) virtual 키워드를 사용하면 언제나 생성된 클래스 멤버함수가 호출된다.
	- (핵심) 베이스 클래스의 멤버함수를 사용할지 파생 클래스의 멤버함수를 사용할지 실행 중에 호출를 결정할 수 있다.
	- 실행 중에 어떤 함수를 호출할지 결정하는 것을 동적 바인딩이라 부른다. 당연히 정적 바인딩보다 느림
	- 이를 위해 클래스 마다 모든 가상 멤버함수의 주소를 가지는 가상 테이블이 생성된다.
	- 즉, 개체를 생성할 때마다, 해당 클래스의 가상 테이블 주소가 함께 저장된다.
	- 멤버함수의 가상성은 상속된다.
	- 베이스 클래스에서 가상으로 선언된 멤버함수가 있다면 
		○ 파생 클래스에서 동일한 시그내처를 가진 함수 역시 가상함수이다. virtual 키워드가 안 붙어 있어도!


비 가상 소멸자
	- Animal* cat = new Cat(5, "coco");
	- delete cat;
		○ 가상함수가 아니므로 ~Animal();만 호출된다.
		○ ~Cat() 소멸자는 호출이되지 않아 메모리 누수가 일어난다.
		○ 그래서 가상 소멸자를 적용해야한다. (중요!)


가상(virtual) 소멸자
	- Animal* cat = new Cat(5, "coco");
	- delete cat;
	- 타입이 Animal*이라해도 가상함수 키워드를 사용했기 때문에
          Cat 개체를 만들어도 Cat 가상테이블 주소를 가지기 때문에 똑같이 행동한다.
	- 그래서 모든 소멸자에 언제나 virtual 키워드를 붙인다.
	- C++14/17에는 해결책 있음
	- 클래스를 만들었는데 상속을 받지 않고,
	- 상속을 받았지만 새로운 메모리 할당을 받지 않았다면
	- 가상함수는 필요없다.


다중 상속
	- 다중 상속일 경우 생성자는 파생 클래스에서 등장한 부모 클래스 순서대로 호출된다.
	- 다중 상속할 경우 다이아몬드 문제가 발생할 수 있다
	- 그래서 다중 상속을 최대한 쓰지 말아야 한다.
	- 대신 인터페이스를 사용하자!!!!!!!!!


순수 가상함수 (추상 클래스) (인터페이스) 
	- virtual void Speak() = 0;
	- 베이스 클래스에서 구현은 하지않고 파생 클래스에서 반드시 구현해야한다.
	- 순수 가상함수를 가지고 있는 베이스 클래스를 추상 클래스라고 한다.
	- 추상 클래스에서는 개체를 만들 수 없고 포인터나 참조형으로 사용 한다.
	- 인터페이스를 흉내 낼 수 있다. (순수 추상 클래스)
		○ 순수 가상함수만 가짐
		○ 멤버 변수는 없음
```


