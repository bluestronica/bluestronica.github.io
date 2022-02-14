### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 스마트(Smart) 포인터 2
- #### 자동 메모리 관리
    - 주로 쓰는 두 가지 기법이 있다
    - 가비지 컬렉션(Garbage Collection, GC), Java와 C#에서 지원
    - 참조 카운팅(Reference Counting, RefCounting), Swift와 애플 Objective-C에서 지원

- #### 가비지 컬렉션
    - 보통 트레이싱 가비지 컬렉션(Tracing Garbage Collection)을 의미
    - 메모리 누수를 막으려는 시도
    - 주기적으로 컬렉션 실행
    - 충분한 여유 메모리가 없을 때 컬렉션이 실행 됨
        - 스케쥴에 따라 또는 수동으로 실행가능
    - 매 주기마다, GC는 루트("root")를 확인한다.
        - 전역 변수
        - 스택
        - 레지스터
    - 힙에 있는 개체에 루트를 통해 접근할 수 있는지 판단
    - 접근할 수 없다면, 가비지로 간주해서 해제
    - 가비지 컬렉션의 문제점
        - 사용되지 않는 메모리를 즉시 정리하지 않음
        - GC가 메모리를 해제해야 하는지 판단하는 동안 애플리케이션이 멈추거나 버벅일 수 있음

- #### 참조 카운팅
    - 가비지 컬렉션처럼, 개체에 대한 참조가 없을 때 개체가 해제됨
    - 언제든 참조 횟수를 활용해서 특정개체가 몇 번이나 참조되고 있는지 판단 가능
    - 어떤 개체 A를 다른 개체 B가 참조할 때 횟수가 늘어남
    - B가 참조를 그만둘 때 횟수 줄어듦
        - B가 범위(scope)를 벗어나는 경우
    - 참조 카운팅은 어떻게 작동할까?
        ```c++
        class StringValue
        {
        private:
            int mRefCount;
            char* mString;
        };

        class MyString
        {
        private:
            StringValue* mStringValue;
        };
        ```
        ```c++
        MyString cat("Coco");          // mStringValue[1, Coco]
        MyString anotherCat("Lulu");   // mStringValue[1, Lulu]

        anotherCat = cat;              // mStringValue[2, Coco]

        // mStringValue를 cat, anotherCat이 참조하기때문에 참조횟수(mRefCount)는 2
        ```

- #### 수동 참조 카운팅
    - COM(즉 DirectX)이 수동 참조 카운딩을 지원
    - std::shared_ptr는 이걸 자동으로 해 줌
        ```c++
        // Person.h
        #include <iostream>
        #include <string>
        class Person
        {
        public:
            unsigned int AddRef();
            Unsigned int Release();
            Person(Const std::string& name);
            Person() = default;
        private:
            ~Person();
            unsigned int mRefCount;
            std::string mName;
        };
        ```
        ```c++
        // Person.cpp
        #include "Person.h"

        unsigned int Person::AddRef()
        {
            ++mRefCount;
            return mRefCount;
        }

        unsigned int Person::Release()
        {
            --mRefCount;
            if (mRefCount == 0)
            {
                delete this;
            }

            return mRefCount;
        }

        Person::Person(const std::string& name)
            : mRefCount(1)
            , mName(name)
        {
            std::cout << mName << " is created." << std::endl;
        }

        Person::~Person()
        {
            std::cout << mName << " is destroyed." << std::endl;
        }
        ```
        ```c++
        // Main.cpp
        #include "Person.h"

        int main()
        {
            Person* person = new Person("Coco");
            Person* copiedPerson = person;
            copiedPerson->AddRef();

            person->Release();
            person = nullptr;

            copiedPerson->Release();
            copiedPerson = nullptr;

            return 0;
        }
        ```

- #### 강한(strong) 참조
    - 강한 참조란 개체 A가 개체 B를 참조할 때, 개체 B는 절대 소멸되지 않음을 의미
    - 강한 참조의 수를 저장하기 위해 강한 참조 카운트를 사용
    - 일반적으르로 새 인스턴스, 즉 개체에 대한 참조를 만들 때 강한 참조 횟수가 늘어남
    - 강한 참조 횟수가 0이 될 때 해당 개체는 소멸됨

- #### 참조 카운팅의 문제점
    - 참조 횟수는 너무 자주 바뀜
        - 멀티 쓰레드 환경에서 안전하려면, lock이나 원자적(atomic) 연산이 필요
        - ++mRefCount 보다 확연히 느림
    - 순환(circular) 참조
        - 개체 A가 개체 B를 참조
        - 개체 B가 개체 A를 참조
        - 절대 해제되지 않음!
        - c++에 해결책이 있음

- #### 가비지 컬렉션 vs 참조 카운팅
    - 가비지 컬렉션
        - 사용하기 확실히 더 쉬움
        - 실시간 또는 고성능 프로그램에 적합하지 않음
    - 참조 카운팅
        - 여전히 사용하기 쉬움
        - 실시간 또는 고성능 프로그램에 적합
        - 멀티 스레드 환경에서는 순수한 포인터보다 훨씬 느림