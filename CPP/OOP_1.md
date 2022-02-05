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
    - 힙(heap) 메모리에 할당된 개체 제거하기
        - delete b;

- #### 개체 배열(Array)
    - Vector* list = new Vector[10];
        - 10개의 Vector 개체를 힙에 만든다.
    - Vector** list = new Vector*[10];
        - 10개의 포인터를 힙에 만든다.

- #### 개체 소멸
    ```C++
    Vector* a = new Vector;
    Vecotr* list = new Vector[10];
    // ...

    delete a;  // 메모리가 즉시 해제 됨
    a = NULL;  // 안 해도 된다.

    delete[] list;  // []를 반드시 넣을 것
    list = NULL;    // 안해도 된다.
    ```