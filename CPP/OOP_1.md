### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 1
- #### OOP C++
    - 클래스
    - 개체
    - 생성자
    - 함수 오버로딩
    - 힙에 개체 생성하기
    - 스택에 개체 생성하기
    - 복사 생성자
    - 소멸자
    - 연산자 오버로딩

- #### 보통 접근 제어자(Access Modifier) 별로 C++ 멤버들을 그룹 지음
    ```C++
    class SomeClass
    {
        public:
            int PublicMember;
        protected:
            int mProtectedMember;
        private:
            int mPrivateMember1;
            int mPrivateMember2;
    }
    ```

- #### 개체 생성
    - 스택(stack)에 메모리에 만들기 (빠름)
        - Vector a;
            - 변수 a는 스택에 Vector size 크기로 공간이 만들어진다.
    - 힙(heap) 메모리에 만들기 (느림)
        - Vector* b = new Vector();
            - 변수 b는 스텍에 존재하면 힙에 만들어진 개체의 시작주소를 가진다.

- #### 개체 배열(Array)
    - Vector* list = new Vector[10];
        - 10개의 Vector 개체를 힙에 만든다.
    - Vector** list = new Vector*[10];
        - 10개의 포인터를 힙에 만든다.

- #### 개체 소멸
    - new를 사용한 뒤에는 꼭 delete를!!!!
    - 메모리 누수 방지를 위해 delete가 필수
        ```C++
        Vector* a = new Vector;
        Vecotr* list = new Vector[10];
        // ...

        delete a;  // 메모리가 즉시 해제 됨
        a = NULL;  // 안 해도 된다.

        delete[] list;  // []를 반드시 넣을 것
        list = NULL;    // 안해도 된다.
        ```
- #### 초기화와 대입의 차이를 이해한다.
    - **초기화 리스트**
        - 실제 개체가 만들어질 때 초기화하는 것을 의미한다.
        - 멤버 변수를 대입(만들어진 후 값을 대입하는 행위) 없이 초기화
        - 상수(const)나 참조변수도 초기화 가능
            - 상수는 한번 초기화 되면 바꿀 수 없다.
            - 참조변수는 처음 만들 때 별칭이 정의가 되어있어야 한다.
            - 그래서 **상수나 참조변수의 경우는 초기화가 아니라 대입을 하면 에러가 난다.**
        ```C++
        class Vector
        {
        public:
            Vector()
                : mX(0)  // 콜론(:)은 초기화 리스트를 시작하겠다는 의미이다.
                , mY(0)
            {                        
            }
        private:
            int mX;
            int mY;
        };
        ```
    - **대입**
        - 대입은 초기화가 된 이후에 실행되는 행위
        ```C++
        class Vector
        {
        public:
            Vector()
            {       
                mX = 0;
                mY = 0;                 
            }
        private:
            int mX;
            int mY;
        };
        ```

- #### 더 나은 Vector 클래스를 만들어 보자
    - **Header 파일**
    ```c++
    // vector.h
    class Vector  // 클래스의 정의
    {
    public:
        Vector();
        Vector(int x, int y);
    private:
        int mX;
        int mY;
    };
    ```
    - **Cpp 파일**
    ```c++
    // vector.cpp
    Vector::Vector()  // 클래스 구현
        : mX(0)
        , mY(0)
    {
    }
    
    Vector::Vector(int x, int y)
        : mX(x)
        , mY(y)
    {
    }
    ```

- #### 기본 생성자
    - Vector() {}
    - 기본 생성자는 매개변수를 받지 않음
    - 클래스에 생성자가 없으면 `컴파일러`가 기본 생성자를 `자동`으로 만들어 준다.
    - 이렇게 자동적으로 만들어지는 생성자는 
        - 멤버 변수를 초기화하지 않는다.
        - 하지만 모든 포함된 개체의 생성자를 호출한다.
    - 생성자가 하나라도 있을 경우 `컴파일러`는 `기본생성자를 생성`하지 않는다.
        ```C++
        class Vector
        {
        public:
            Vector(int x, int y);
        private:
            int mX;
            int mY;
        };

        void Foo()
        {
            Vector a;          // 컴파일 에러 : 컴파일러가 기본생성자를 생성하지 않았기 때문.
            Vector b(10, 20);  // OK
        }
        ```

- #### 생성자 오버로딩(Overloading)
    - 여러 개의 생성자를 만들 수 있다.
    - 기본 생성자
        ```C++
        Vector() : mX(0), mY(0) {}
        Vector a;  // 기본 생성자 호출
        ```
    - 매개변수를 가지는 생성자
        ```C++
        // 2개의 매개변수를 가지는 생성자
        Vector(int x, int y)
            : mX(x)
            , mY(y)
        {            
        }

        Vector a(1, 3);  // 매개변수 목록이 일치하는 생성자 호출
        ```

- #### 소멸자(Destructor)
    - 소멸자는 클래스 안에서 동적으로 할당된 메모리 해제를 위한 것이다.
        - C++ 클래스는 그 안에서 동적으로 메모리를 할당할 수도 있다.
        - 그런 경우 필히 `소멸자에서 메모리를 직접 해제`해 줘야 한다.
    - 개체가 지워질 때 소멸자가 호출되어 처리한다.
        ```C++
        // MyString.h
        Class MyString
        {
        public:
            Mystring();
            ~MyString();
        private:
            char* mChars;
            int mLength;
            int mCapacity;
        };
        ```
        ```C++
        // MyString.cpp
        MyString::MyString()
            : mLength(0)
            , mCapacity(15)
        {
            mChars = new char[mCapacity + 1];  // 클래스 안에서 동적으로 할당된 메모리는
        }

        MyString::~MyString()    // 개체가 지워질 때 소멸자가 호출되어
        {
            delete[] mChars;     // 메모리 해제를 한다.
            // mCapacity = 0;
            // mChars = NULL;
        }
        ```

- #### 중괄호 초기화
    - 장점
        - vector등 container와 잘 어울린다.
        - 축소 변환 방 
        ```C++
        vector<int> v1{ 9, 10, 11 };   // push_back 와 똑같
        map<int, string> m1{ {1, "a"}, {2, "b"} };
        string s{ 'a', 'b', 'c' };
        regex rgx{ 'x', 'y', 'z' };
        ```


- #### 클래스의 멤버 함수
    ```C++
    // Vector.h
    class Vector
    {
    public:
        void SetX(int x);
        void SetY(int y);
        int GetX() const;
        int GetY() const;
        void Add(const Vector& other);
    private:
        int mX;
        int mY;
    }
    ```

- #### const 멤버 함수
    - 맴버 함수 내에서 모든 멤버 변수가 변하는 것을 방지(상수화)
        ```C++
        int GetX() const
        {
            return mX;     // OK
        }

        void AddConst(const Vector& other) const
        {
            mX = mX + ohter.mX;   // 컴파일 에러
            mY = mY + ohter.mY;   // 컴파일 에러 
        }
        ```
- #### const 멤버 변수
    - const 변수는 반드시 선언 시 초기화 해야한다.
    - class의 멤버 변수를 const로 선언 시에는 반드시 초기화 리스트를 사용해야한다.
    ```C++
    class Foo
    {
    	const int num;    // 메모리 할당이 아님
     
    	Foo(void)
    		: num(1)    // const int num = 1; 초기화 리스트 사용
    	{
    	}
    };
     
    class Bar
    {
    	const int num;
     
    	Bar(void)
    	{
    		num = 1;    // Compile Error
    		// const int num;
    		// num = 1;
    	}
    };
    ```
    
- #### const 포인터 변수
    - 값(*ptr)을 상수화
    ```C++
    int num = 1;
    const int* ptr = &num;    // *ptr을 상수화

    *ptr = 2;     // compile error
    num = 2;     // pass
    ```

    - 포인터 변수(ptr)를 상수화
    ```C++
    int num1 = 1;
    int num2 = 2;
    int* const ptr = &num1;    // ptr을 상수화

    ptr = &num2;     // compile error
    ```

- #### 구조체에 관한 코딩표준
    - C++에서는 구조체를 클래스처럼 쓸 수 있음
        - 하지만 절대 그러지 말 것
        - 구조체는 `C 스타일`로 쓰자!
    - struct는 순수하게 데이터뿐이여야 한다.(Plain Old Data, POD)
        - 사용자가 선언한 생성자는 소멸자 X
        - static 아니 private/protected 멤버 변수 X
        - 가상함수 X
        - 메모리 카피가 가능
            - memcpy()를 사용하여 struct를 char[]로, 혹은 반대로 복사할 수 있음
        



















