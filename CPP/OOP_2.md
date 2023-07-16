### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 2
- #### 복사 생성자
    - 같은 클래스에 속한 다른 개체를 이용하여 새로운 개체를 초기화
    - 즉, 다른 개체의 값을 새로운 개체에 그대로 복사
      ```C++
      // Vector.h
      class Vector                    // 만들려는 새로운 개체에
      {
      public:
      	Vector(const Vector& other);  // 다른 개체를 가져와서
      private:
      	int mX;
      	int mY;
      };

      // Vector.cpp
      Vector::Vector(const Vector& other)  // 복사 생성자로
      	: mX(other.mX)                // 값을 새로운 개체에 복사
      	, my(other.mY)
      {
      }
      ```
    - a를 가져다가 b를 초기화
        ```C++
        Vector(const Vector& other);

        Vector a;     // 매개변수 없는 생성자를 호출
        Vector b(a);  // 복사 생성자를 호출
        ```
    - 암시적(implicit) 복사 생성자는 `얕은 복사(shallow copy)`를 수행한다.
        - 코드에 복사 생성자가 없는 경우, `컴파일러가 암시적 복사 생성자를 자동 생성`한다.
        - 멤버 별 복사
        - 각 멤버의 값을 복사한다.
        - 개체인 멤버변수는 `그 개체의 복사 생성자`가 호출된다.
    - 사용자가 직접 복사 생성자를 만들어서 `깊은 복사(deep copy)`를 한다.
        - 클래스 안에서 동적으로 메모리를 할당하고 있다면
        - 얕은 복사가 위험할 가능성이 매우 높다.
        - 그래서 직접 복사 생성자를 만들어서 깊은 복사를 해야한다.
        - 즉, 포인터 변수가 가리키는 실제 데이터까지도 복사
            ```C++
            // ClassRecord.cpp
            // 매개변수를 가지는 생성자
            ClassRecord::ClassRecord(const int* scores, int count)
                : mCount(count)
            {
                mScores = new int[mCount];
                memcpy(mScores, scores, mCount * sizeof(int));
            }

            // 직접 만든 복사 생성자
            ClassRecord::ClassRecord(const ClassRecord& other)
                : mCount(other.mCount)
            {
                mScores = new int[mCount];
                memcpy(mScores, other.mScores, mCount * sizeof(int));
            }

            // 암시적 복사 생성자처럼 일방적인 값 대입(포인터의 주소만 복사되는 현상)이 아니라
            // 직접 작성한 복사 생성자에 new 즉 힙에 새로운 공간을 만들어 값들을 깊은 복사해온다.
            ```
            ```C++
            // Main.cpp
            // scores에 5개의 점수가 들어있다고 가정하자
            ClassRecord classRecord(scores, count);
            ClassRecord* classRecordCopy = new ClassRecord(classRecord);
            delete classRecordCopy;
            ```
    - Sample Code - Copy Constructor With char Array
        ```c++
        // String.h
        class String
        {
        public:
            String(const char* str);     // 매개변수 1개 받는 생성자
            String(const String& str);   // 복사 생성자
            ~String();                   // 소멸자

            void Print();                // 멤버 함수

        private:
            char* mString;      // 클래스안에서 힙에 동적으로 할당하는 private 멤버 변수 
            size_t mSize;
        };
        ```
        ```c++
        // String.cpp
        String::String(const char* str)   // 매개변수 1개 받는 생성자
            : mSize(strlen(str) + 1)      // 초기화 리스트
        {
            // 스택에 생성된 mString 포인터 변수는 힙에 할당된 메모리 시작 주소를 담는다.
            mString = new char[mSize];  // 클래스 안에서 mSize 크기로 동적 메모리 할당
            memcpy(mString, str, mSize); // str의 값을 할당받은 공간에 복사한다.
        }

        String::String(const String& str)   // 복사 생성자
            : mSize(str.mSize)
        {
            mString = new char[mSize];
            memcpy(mString, str.mString, mSize);
        }

        String::~String()   // 소멸자
        {
            delete[] mString;   // 클래스 안에서 할당받은 메모리를 직접 해제
        }

        void String::Print()
        {
            cout << "Member string : " << mString << endl;
        }
        ```
        ```c++
        // Main.cpp
        String myName(" John Doe");  // 생성자 호출, myName 개체는 스택에 생성
		String myNameCopy(myName);   // 복사 생성자 호출

		myName.Print();
		myNameCopy.Print();
        ```

- #### 클래스 안에서 동적으로 메모리를 할당하고 있다면 직접 메모리 해제를 하는 코드 작성해야한다.
    - 소멸자 작성
        - 클래스에서 동적으로 할당된 메모리 해제를 위한 것
    - 복사 생성자 작성
        - 깊은 복사를 위한 것
    - 대입 연산자 작성
        - 복사 생성자를 구현했다면 대입 연산자도 구현해야 한다.

- #### 함수(메서드) 오버로딩
    - 매개변수 목록을 제외하고는 모든 게 동일
    - 반환형은 상관 없음
        ```C++
        void Print(int score);                       // OK
        void Print(const char* name);                // OK
        void Print(float gpa, const char* name);     // OK
        int Print(int score);                        // 컴파일 에러 : 반환형은 상관 없음.. 매개변수 목록이 일치해서 에러
        int Print(float gpa);                        // OK
        ```
    - Sample Code - Vector Class with Function Overloading
        ```c++
        // Vector.cpp
        Vector::Vector()       // 기본 생성자
            : Vector(0, 0)     // 초기화 리스트
        {
        }

        Vector::Vector(int x, int y)   // 매개변수 2개인 생성자
            : mX(x)                    // 초기화 리스트
            , mY(y)
        {
        }

        int Vector::GetX() const
        {
            return mX;
        }

        void Vector::SetX(int x)
        {
            mX = x;
        }

        void Vector::SetY(int y)
        {
            mY = y;
        }

        int Vector::GetY() const
        {
            return mY;
        }

        bool Vector::IsEqual(const Vector& v) const
        {
            return (mX == v.mX && mY == v.mY);
        }

        Vector Vector::Multiply(const Vector& v) const
        {
            Vector result(mX * v.GetX(), mY * v.GetY());

            return result;
        }

        Vector Vector::Multiply(int multiplier) const
        {
            Vector result(mX * multiplier, mY * multiplier);

            return result;
        }

        void Vector::Scale(const Vector& v) 
        {
            mX *= v.mX;
            mY *= v.mY;
        }

        void Vector::Scale(int multiplier)
        {
            mX *= multiplier;
            mY *= multiplier;
        }
        ```
        ```c++
        // Main.c
        Vector vector1(3, 5);
		Vector vector2(7, 9);
		const int multiplier = 3;

		Vector result = vector1.Multiply(vector2);
		result = vector1.Multiply(multiplier);

		vector1.Scale(vector2);
		vector1.Scale(multiplier);
        ```

- #### 연산자
    - 함수처럼 작동하는 부호
    - C++에서는 프로그래머가 연산자를 오버로딩할 수 있다.

- #### 멤버 함수를 이용한 연산자 오버로딩
    - Sample Code - Vector2 Class with Operator Overloading
        ```C++
        // Vector2.cpp
        Vector2::Vector2()
            : Vector2(0, 0)
        {
        }

        Vector2::Vector2(int x, int y)
            : mX(x)
            , mY(y)
        {
        }

        int Vector2::GetX() const
        {
            return mX;
        }

        void Vector2::SetX(int x)
        {
            mX = x;
        }

        void Vector2::SetY(int y)
        {
            mY = y;
        }

        int Vector2::GetY() const
        {
            return mY;
        }

        bool Vector2::operator==(const Vector2& rhs) const
        {
            return (mX == rhs.mX && mY == rhs.mY);
        }

        Vector2 Vector2::operator*(const Vector2& rhs) const
        {
            Vector2 result(mX * rhs.mX, mY * rhs.mY);

            return result;
        }

        Vector2 Vector2::operator*(int multiplier) const
        {
            Vector2 result(mX * multiplier, mY * multiplier);

            return result;
        }

        Vector2 operator*(int multiplier, const Vector2& v)
        {
            Vector2 result(v.mX * multiplier, v.mY * multiplier);

            return result;
        }

        Vector2& Vector2::operator*=(const Vector2& rhs)
        {
            mX *= rhs.mX;
            mY *= rhs.mY;

            return *this;
        }

        Vector2& Vector2::operator*=(int multiplier)
        {
            mX *= multiplier;
            mY *= multiplier;

            return *this;
        }

        std::ostream& operator<<(std::ostream& out, const Vector2& v)
        {
            out << v.mX << ", " << v.mY << std::endl;

            return out;
        }
        ```
        ```c++
        // Main.c
        Vector2 vector1(3, 5);
		Vector2 vector2(7, 9);

		const int multiplier = 3;

		Vector2 result = vector1 * vector2;
		result = vector1 * multiplier;
		result = multiplier * vector1;

		vector1 *= vector2;
		vector1 *= multiplier;
        ```

- #### friend 키워드
    - 다른 클래스나 함수가 나의 private 또는 protected 멤버에 접근할 수 있게 허용
    - friend 클래스
        ```C++
        // X.h
        class X
        {
            friend class Y;  // Y 클래스가 나의 private 또는 protected 멤버에 접근할 수 있게 허용
        private:
            int mPrivateInt;
        };
        ```
        ```C++
        // Y.h
        #include "X.h"
        class Y
        {
        public:
            void Foo(X& x);
        };
        ```
        ```C++
        // Y.cpp
        void Y::Foo(X& x)
        {
            x.mPrivateInt += 10;   // OK  // X 클래스 private 멤버에 접근이 가능
        }
        ```
    - friend 함수
        - friend 함수는 멤버 함수가 아니다
        - 하지만 다른 클래스의 private 멤버에 접근할 수 있다.
        ```C++
        class X
        {
            friend void Foo(X& x);  // 함수에서 나의 private 또는 protected 멤버에 접근할 수 있게 허용
        private:
            int mPrivateInt;
        };
        ```
        ```C++
        // GlobalFunction.cpp
        void Foo(X& x)
        {
            x.mPrivateInt += 10;   // OK
        }
        ```

- #### 멤버 아닌 함수를 이용한 연산자 오버로딩 
    - 멤버 아닌 함수, friend 함수를 이용해 연산자 오버로딩을 작성한다.
        ```C++
        // Vector.h
        class Vector
        {
            friend std::ostream& operator<<(const std::ostream& os, const Vector& rhs);
        public:
            // ...
        private:
            // ...
        };
        ```
        ```C++
        // Vector.cpp
        std::ostream& operator<<(std::ostream& os, const Vector& rhs)
        {
            os << rhs.mX << ", " << rhs.mY;
            return os;
        }
        ```
        ```C++
        // main.cpp
        Vector vector1(10, 20);
        std::cout << vector1 << std::endl;
        ```

- #### 연산자 오버로딩과 const, &
    ```C++
    Vector operator+(const Vector& rhs) const;  // 멤버 변수의 값이 바뀌는 것을 방지 하기위해 const 사용
    std::ostream& operator<<(std::ostream& os, const Vector& rhs);
    ```
    - const를 쓰는 이유
        - 멤버 변수의 값이 바뀌는 것을 방지
        - 최대한 많은 곳에 const 붙일 것!
        - 지역(local) 변수 까지도
    - const &를 사용하는 이유
        - 불필요한 개체의 사본이 생기는 것을 방지
        - 멤버 변수가 바뀌는 것도 방지
    - const를 사용하지 않는 경우도 있다.
        ```C++
        vector1 += vector2;
        vector1 = vector1.operator+=(vector2);

        Vector& operator+=(const Vector& rhs);

        // Vector.cpp
        Vector& Vector::operator+=(const Vector& rhs)
        {
            mX += rhs.mX;
            mY += rhs.mY;

            return *this;
        }
        ```
    - & 사용하면 무슨 일이 일어나나?
        ```C++
        // Vector.cpp
        Vector Vector::operator+(Vector& rhs) // v2 별칭(내부적으로 v2 주소 2048을 가진다.)
        {
            Vector r;

            r.mX = this->mX + rhs.mX;     // 값으로 멤버 접근 rhs.mX
            r.mY = this->mY + rhs.mY;     // 포인터로 멤접근 this->mY

            return r;  // r 개체를 result 개체에 대입, 값 복사한다.
        }   // operator+() 함수 영역을 빠져나오면서 그 영역의 데이터는 가비지상태가 된다.
        ```
        ```C++
        // main.cpp
        Vector v1(10, 20);
        Vector v2(30, 40);   // v2 개체의 스택 주소는 2048이다.

        Vector result = v1 + v2;
        ```

- #### 대입(assignment) 연산자
    - operator=
    - a를 가져다가 b에 대입 :  b = a;
        - 단, b는 생성되는 과정이 아니라 이미 존재하던 개체
    - 복사 생성자와 거의 동일
        - 그러나 대입 연산자는 메모리를 해제해 줄 필요가 있을 수도..
        - 왜냐하면 b는 이미 존재하던 개체이므로, b가 가지고 있던 힙 메모리를 버려야 한다.
        - 다 지워버리고 b에 맞는 메모리를 만들어 memcpy 들어가야한다.
    - 복사 생성자를 구현했다면 대입 연산자도 구현해야 한다.
    - 암시적 operator=
        - operator= 구현이 안 되어 있으면 컴파일러가 operator= 연산자를 자동으로 만들어 준다.

- #### 클래스에 딸려오는 기본 함수들 (컴파일러의 암시적 생성)
    - 매개변수 없는 생성자
    - 복사 생성자
    - 소멸자
    - 대입(=) 연산자

- #### 클래스에 딸려오는 기본 함수들 `지우는` 법
