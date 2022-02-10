### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 인라인 함수(Inline Function)
- #### 함수 호출할 때
    - 함수는 메모리 안에 "할당"되어 있다
    - 함수를 호출하기 위해 필요한 단계들
        1. 변수들을 스택에 푸쉬(push)
        2. 함수 주소로 점프
        3. 함수를 실행
        4. 호출자 함수로 다시 점프
        5. 1번 단계에서 넣어뒀던 변수들을 "pop"
    - 이처럼 여러 단계가 존재한다.
    - 연산 그 자체가 매우 단순한 함수를 쓸 때
    - 함수 호출에 필요한 오버헤드를 떠맡기에는 좀 부담이 된다
    - 그때 그 해결법으로 **인라인 함수**를 쓴다.

- #### 인라인 함수의 동작 원리
    - 멤버 아닌 **인라인 함수**
        - 매크로(MACRO)와 매우 비슷한 개념 : #define
            ```c++
            inline int Square(int number)
            {
                return number * number;
            }
            
            int main()
            {
                int number = 2;
                int result = Square(number);

                // 컴파일 과정을 거치면                
                // int result = number * number;
                // 함수 호출이 되는 것이 아니라
                // 매크로(MACRO)와 매우 비슷한 개념으로 실행된다.

                return 0;
            }
            ```
    - **인라인 멤버 함수**
        - 함수호출 대신에 사실상 복붙과 비슷
            ```c++
            class Animal
            {
            public:
                inline int GetAge();
            };

            int Animal::GetAge()
            {
                return mAge;
            }
            ```
            ```c++
            // main.cpp
            Cat* myCat = new Cat(2, "Coco");
            int age = myCat->GetAge();

            // 컴파일 과정을 거치면
            // int age = myCat->mAge;
            ```
        
- #### 인라인 함수를 쓸 때 주의점
    - inline 키워드는 힌트일 뿐
        - 실제로는 인라인이 안될 수도 있음
        - 컴파일러가 자기 맘대로 아무 함수나 인라인 할 수도 있음...
    - 인라인 함수 구현이 **헤더 파일에 위치**해야 함
        - 복붙을 하려면 컴파일러가 그 구현체를 볼 수 있어야 하기때문에...
        - 각 cpp 파일은 따로 컴파일된다.
        - 따라서 b.h를 inlcude 하는 a.cpp파일을 컴파일할 때, 컴파일러는 b.cpp에 뭐가 들어 있는지 모른다.
    - 간단한 함수에 적합
        - 특히 getter나 setter에...
    - 실행파일의 크기가 증가하기 쉬움
        - 동일한 코드를 여러 번 복붙하니까
        - 남용하지 말자!
        - 실행파일이 작을수록 CPU 캐시하고 잘 작동 -> 속도가 빨라질 수 있음

- #### Sample code
    ```c++
    // Vector.h
    	class Vector
	{
	public:
		Vector();
		Vector(int x, int y);

		inline int GetX() const;
		inline void SetX(int x);

		inline int GetY() const;
		inline void SetY(int y);

		inline bool IsEqual(const Vector& v) const;
		inline Vector Multiply(const Vector& v) const;
		inline Vector Multiply(int multiplier) const;

		inline void Scale(const Vector& v);
		inline void Scale(int multiplier);

	private:
		int mX;
		int mY;
	};

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
    // Vector2.h
    	class Vector2
	{
	public:
		Vector2();
		Vector2(int x, int y);

		inline int GetX() const;
		inline void SetX(int x);

		inline int GetY() const;
		inline void SetY(int y);

		inline bool operator==(const Vector2& rhs) const;

		inline Vector2 operator*(const Vector2& rhs) const;
		inline Vector2 operator*(int multiplier) const;
		friend inline Vector2 operator*(int multiplier, const Vector2& v);

		inline Vector2& operator*=(const Vector2& rhs);
		inline Vector2& operator*=(int multiplier);

		friend inline std::ostream& operator<<(std::ostream& out, const Vector2& vector);

	private:
		int mX;
		int mY;
	};

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

	int Vector2::GetX() const
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
