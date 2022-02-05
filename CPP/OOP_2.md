### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 개체지향 프로그래밍 2
- #### 복사 생성자
    - 같은 클래스에 속한 다른 개체를 이용하여 새로운 개체를 초기화
    - 즉, 다른 개체의 값을 새로운 개체에 그대로 복사
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