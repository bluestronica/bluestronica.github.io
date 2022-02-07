### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### static 키워드
- #### static
    - 파일 속, 네임스페이스 속, 클래스 속, 함수 속
    - 범위(scope)의 제한을 받는 전역 변수

- #### 정적(static) 변수
    - 멤버 아닌 **함수 안의** 정적 변수
        - 함수로 범위(scope)가 제한된 전역 변수
            ```c++
            void Accumulate(int number)
            {   // 이 영역은 result에 접근 가능
                static int result = 0;      // 2, result는 함수 안의 정적 변수
                result += number;           // 3, 6
                std::cout << "result = " << result << std::endl; // 4, 7
            }

            int main()
            {   // 이 영역은 result에 접근 불가
                Accumulate(10);   // 1, 10
                Accumulate(20);   // 5, 30

                return 0;
            }
            ```

- #### 정적(static) 멤버 변수
    - 클래스당 **하나의 COPY만** 존재
        - 각 개체마다 정적 멤버 변수가 생기는 것이 아니다.
        - 개체의 메모리 레이아웃의 일부가 아님
    - 클래스 메모리 레이아웃에 포함
    - exe파일 안에 필요한 메모리가 잡혀 있음
        ```c++
        // Cat.h
        class Cat
        {
        public:
            Cat(int age, const string& name);
        private:
            static int mCount;
        };
        ```
        ```c++
        // Cat.cpp
        int Cat::mCount = 0;
        Cat::Cat(int age, const string& name)
        {
            ++mCount;
        }
        ```
        ```c++
        // Main.cpp
        Cat* myCat = new Cat(2, "Coco");
        Cat* yourCat = new Cat(5, "Momo");
        Cat* hisCat = new Cat(3, "Nana");
        Cat* herCat = new Cat(7, "Coco");

        // 메모리에 mCount 변수가 몇개 일까? 오직 한 개만 존재!!!!

        // mCount의 값은? 4
        ```
- #### 정적(static) 멤버 변수 베스트 프랙티스
    - 함수안에 정적 변수를 넣지 말 것!
        - **클래스 안에 넣을 것!**
    - 전역변수 대신 정적 멤버 변수를 쓸 것
        - 범위를 제한하기 위해
    - C스타일의 정적변수를 쓸 이유가 이제 없음!

- #### 정적(static) 멤버 함수
    - 논리적인 범위에 제한 된 전역 함수
    - 해당 클래스의 정적 멤버에만 접근 가능
    - 개체가 없어도 정적 함수를 호출할 수 있음
        - Math::Square(10);