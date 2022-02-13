### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 템플릿(Template) 프로그래밍
- #### 템플릿이란?
    - c#에서의 제네릭 메서드/클래스와 비슷    
    - 덕분에 코드를 자료형마다 중복으로 작성하지 않아도 된다.
    - 컴파일 시에 컴파일러가 자동으로 만들어주는 것을 템플릿 프로그래밍이라 한다.

- #### 함수 템플릿
    ```c++
    // MyMath.h
    template <typename T>  // 또는 template <class T> : 차이 없음. 그냥 typename을 사용하자!
    T Add(T a, T b)
    {
        return a + b;
    }
    ```
    ```c++
    // Main.cpp
    std::cout << Add<int>(3, 10) < std::endl;
    std::cout << Add<float>(3.14f, 10.14f) < std::endl;
    ```

- #### 템플릿은 어떻게 작동할까?
    - 템플릿을 인스턴스화할 때마다
    - 컴파일러가 내부적으로 코드를 생성
    - 그래서, 템플릿에 넣는 자료형 가짓수에 비례해서 exe파일 크기 증가한다.
    - 그리고, 컴파일 타임에 어느 정도 다형성을 부여할 수 있음
        - 가독성이 많이 떨어짐.. 비추

- #### 선언과 구현체가 헤더파일 한 곳에 넣는다.
    - 컴파일러는 (MyArray.h와 Main.cpp)
    - (MyArray.h와 MyArray.cpp) 이렇게 따로 컴파일한다.
    - 그래서 컴파일러가 Main.cpp을 컴파일할 때, MyArry.cpp를 못 찾음
    - 왜냐하면, Main.cpp에서는 타입(int)이 정해져있는데
    - MyArray.h는 타입이 정해져 있지 않다. 타입이 정해지지 않은 상태에서
    - 구현체가 있는 MyArray.cpp에서 타입을 모르니 `MyArray<int>`를 만들어 줄 수 없다.
    - 이런 똑같은 문제를 인라인 함수에서 볼 수 있다.
    - 그거랑 똑같은 해결책으로 해결할 수 있다.
        ```c++
        // MyArray.h
        template<typename T>
        class MyArray
        {
        public:
            bool Add(T data);
            MyArray();
        private:
            enum { MAX = 3 };
            int mSize;
            T mArray[MAX];
        };

        template<typename T>
        inline bool MyArray<T>::Add(T data)
        {
            return false;
        }

        template<typename T>
        inline MYArray<T>::MyArray()
            : mSize(0)
        {            
        }
        ```
    - 선언과 구현체가 헤더파일 한 곳에 넣어 해결한다.
    - 그리고 컴파일이 느려지는 이유가 되기도 한다.
    - ##### 한 개의 템플릿 매개변수
        ```c++
        // MyPair.h
        template<typename T>
        class MyPair
        {
        public:
            const T& GetFirst() const;
            const T& GetSecond() const;
            MyPair(const T& first, const T& second);

        private:
            T mFirst;
            T mSecond;
        };

        template<typename T>
        const T& MyPair<T>::GetFirst() const
        {
            return mFirst;
        }

        template<typename T>
        const T& MyPair<T>::GetSecond() const
        {
            return mSecond;
        }

        template<typename T>
        MyPair<T>::MyPair(const T& first, const T& second)
            : mFirst(first)
            , mSecond(second)
        {            
        }
        ```
        ```c++
        // Main.cpp
        std::vector<MyPair<std::string>> students;

        students.emplace_back(MyPair<std::string>("a1234", "Coco"));
        students.emplace_back(MyPair<std::string>("a5678", "Mocha"));

        for (auto it = students.begin(); it != students.end(); ++it)
        {
            std::cout << int->GetFirst() << " : " << it->GetSecond() << std::endl;
        }
        ```
    - ##### 두 개의 템플릿 매개변수
        ```c++
        // MyPair.h
        template<typename T, typename U>
        class MyPair
        {
        public:
            const T& GetFirst() const;
            const U& GetSecond() const;
            MyPair(const T& first, const U& second);

        private:
            T mFirst;
            U mSecond;
        };

        template<typename T, typename U>
        const T& MyPair<T, U>::GetFirst() const
        {
            return mFirst;
        }

        template<typename T, typename U>
        const U& MyPair<T, U>::GetSecond() const
        {
            return mSecond;
        }

        template<typename T, typename U>
        MyPair<T, U>::MyPair(const T& first, const U& second)
            : mFirst(first)
            , mSecond(second)
        {            
        }
        ```
        ```c++
        // Main.cpp
        std::vector<MyPair<std::string, int>> students;

        students.emplace_back(MyPair<std::string, int>("a1234", "Coco"));
        students.emplace_back(MyPair<std::string, int>("a5678", "Mocha"));

        for (auto it = students.begin(); it != students.end(); ++it)
        {
            std::cout << int->GetFirst() << " : " << it->GetSecond() << std::endl;
        }
        ```

- #### 템플릿 특수화
    - 특정한 템플릿 매개변수를 받도록 템플릿 코드를 커스터마이즈할 수 있다.
    - 전체 템플릿 특수화
        - 템플릿 매개변수 리스트가 비어 있음
            ```c++
            template<typename Val, typename EXP>
            VAL Power(const VAL value, EXP exponent) {}  // 모든 형을 받는 제네릭 power()

            template <>
            float Power(float value, float exp) {}       // float를 받도록 특수화된 power()
            ```
        - sample code
            ```c++
            // MyMath.h
            template<typename V, typename EXP>
            V Power(const V value, EXP exponent)
            {
                V result = 1;
                while (exponent-- > 0)
                {
                    return *= value;
                }

                return result;
            }
            ```
            ```c++
            // Main.cpp
            template<>
            float Power(float value, float exp)
            {
                return std::powf(value, exp);
            }

            int main()
            {
                int powerResultInt = Power(10, 2);
                float powerResultFloat = Power(10.f, 2.5f);
            }
            ```
    - 부분 템플릿 특수화
        ```c++
        template<clas T, class Allocator>
        class std::vector<T, Allocator> {}      // 모든 형을 받는 제네릭 vector

        template<class Allocator>
        class std::vector<bool, Allocator>     // bool을 받도록 특수회된 vector
        ```
        - sample code
            ```c++
            // MyArray.h

            // 정의는 생략
            template<class T>
            class MyArray
            {
            public:
                bool Add(T data);
                MyArray();

            private:
                enum { MAX = 3 };
                int mSize;
                T mArray[MAX];
            };

            template<>
            class MyArrayy<bool>
            {
            public:
                bool Add(bool data);
                MyArray();
            
            private:
                enum { MAX = 3 };
                int mSize;
                int mArray;
            };

            // 구현
            MyArray<bool>::MyArray()
                : mSize(0)
                , mArray(0)
            {                
            }

            bool MyArray<bool>::Add(bool data)
            {
                if (mSize >> MAX)
                {
                    return false;
                }

                if (data)
                {
                    mArray |= (1 << mSize++);
                }
                else
                {
                    mArray &= ~(1 << mSize++);
                }

                return true;
            }
            ```

- #### 장점과 단점
    - 컴파일러가 컴파일 도중에 각 템플릿 인스턴스에 대한 코드를 만들어준다.
        - 컴파일 타임은 비교적 느림
        - 템플릿 매개변수를 추가할수록 더 느려짐
        - 하지만 런타임 속도는 더 빠를지도 모름
        - 그러나 실행파일 크기가 커지기 때문에 항상 그런 것은 아님
    - 자료형만 다른 중복 코드를 없애는 훌륭한 방법이다.
    - 하지만 쓸모없는 템플릿 변형을 막을 방법이 없음
        - 최대한 제네릭 함수를 짦게 유지할 것
        - 제네릭이 아니어도 되는 부분은 별도의 함수로 옮기는 것도 방법
        - 이 함수가 인라인 될 수도 있음
    - 컴파일 도중에 다형성을 부여할 수 있다
        - 가상 테이블이 없어서 프로그램이 더 빠름
        - 하지만 exe 파일이 커지면서 느려질 수 있음
    - 코드 읽기가 더 힘들어졌다.

- 템플릿 프로그래밍 베스트 프랙티스
    - 컨테이너의 경우 매우 적합
        - 아주 다양한 형들을 저장할 수 있음
        - 그런 이유로 java와 C# 제네릭이 주로 컨테이너에 쓰이는 것
    - 컨테이너가 아닌 경우
        - 각기 다른 서넛 이상의 자료형을 다룬다면 템플릿을 쓰자
        - 두 가지 정도라면 그냥 클래스 2개 만들자
            - class Math;
            - class FloatMath;