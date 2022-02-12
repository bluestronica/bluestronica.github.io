### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 표준 템플릿 라이브러리(STL, Standard Template Library) 1
- #### STL 컨테이너 목록
    - 벡터(Vector)
    - 맵(map)
    - unordered_map
    - 셋(set)
    - unordered_set
    - array
    - 스택(stack)
    - 큐(queue)
    - 리스트(list)
    - 덱(디큐, deque)
    - ...

- #### 벡터
    - 저장된 모든 요소들이 연속된 메모리 공간에 위치한 동적 배열
    - vector 변수 만들기 1
        - 빈 vector를 생성한다.
            ```c++
            // std::vector<type><name>;
            std::vector<int> scores
            std::vector<string> names;

            std::vector<Cat> myCats;
            
            std::vector<Cat*> myCats;
            ```
    - vector 변수 만들기 2
        - vector의 사본 만들기
            ```c++
            // std::vector<type><name>(const vector& x);
            std::vector<int> scores;

            scores.push_back(1);  // puhs_back() 제일 마지막에 요소 추가
            scores.push_back(2);

            std::vector<int> scores2(scores);   // scores의 사본

            // 복사 생성자를 호출한다고 생각하면 된다.
            ```
    - 용량(capacity)과 크기(size)
        - vector에 **할당된 요소 공간 수**
            - capacity();
        - vector에 **실제로 들어 있는 요소 수**
            - size();
    - vector의 용량(capacity) 늘리기
        - reserve(<size>);
        - 용량 증가에 따른 메모리 할당과 재할당이 일어나기 때문에 속도가 느려지는 경우가 많고
        - 메모리 파편화 현상이 일어나게 된다.
        - 그래서 **생성 초기에 충분한 공간을 잡기위해**
        - **vector를 생성한 직후에 reserve()함수를 호출하자.**
    - 모든 요소 출력하기 1
        ```c++
        std::vector<int> scores;
        scores.resrve(2);

        scores.push_back(30);
        scores.push_back(50);

        for (size_t i = 0; i < scores.size(); ++i)
        {
            std::cout << scores[i] << " ";
        }
        ```
    - 모든 요소 출력하기 2
        ```c++
        std::vector<int> scores;
        scores.resrve(2);

        scores.push_back(30);
        scores.push_back(50);

        for(std::vector<int>::iterator iter = scores.begin(); iter != scores.end(); ++iter)
        {
            std::cout << *iter << " ";
        }
        ```
    - 반복자(iterator)
        - vector의 요소들을 앞에서부터 순회
            - begin();
                - vector의 첫 번째 요소를 가리키는 반복자를 반환
            - end();
                - vector의 마지막 요소 바로 뒤의 요소를 가리키는 반복자를 반환
        - vector의 요소들을 뒤에서부터 순회
            - rbegin();
                - vector의 마지막 요소를 가리키는 역방향 반복자를 반환
            - rend();
                - vector의 첫 번째 요소 앞의 "요소"를 가리키는 역방향 반복자를 반환
    - 특정 위치에서 요소 삽입하기
        ```c++
        std::vector<int> scores;
        scores.reserve(4);

        scores.push_back(10);      // 10
        scores.push_back(50);      // 10, 50
        socres.push_back(38);      // 10, 50, 38
        scores.push_back(100);     // 10, 50, 38, 100

        std::vector<int>::iterator it = scores.begin();

        // it++;   10, 80, 50, 38, 100
        it = scores.insert(it. 80);  // 80, 10, 50, 38, 100
        ```
        - 복사 문제
            - 80을 insert하기 위해 10, 50, 38을 뒤로 하나씩 밀어냈다.
            - 앞에 값을 하나 넣기 위해 그 뒤의 모든 데이터가 복사 붙여넣기 행위를 하게 되는 것이다.
            - 5메가 데이터에 값을 하나 넣는다고 생각하면 그 뒤의 모든 데이터가 복사 붙여넣기 행위를 하니 문제가 된다.
            - 배열의 고질적인 문제이다.
        - 재할당 + 복사 문제
            - 재할당이 더 큰 문제
            - 10앞에 80을 넣을려고 하니 공간이 4개인데 더 이상 확장할 여지가 없다.
            - 다시 공간을 재할당해서 데이터를 복사해 넣는다. 많은 일이 일어난다.
            - 이래서 reserve()를 사용해서 공간을 미리 미리 잡아 둬야 한다.
    - 특정 위치에 있는 요소 삭제하기
        ```c++
        std::vector<int> scores;
        scores.reserve(4);

        scores.push_back(10);      // 10
        scores.push_back(50);      // 10, 50
        socres.push_back(38);      // 10, 50, 38
        scores.push_back(100);     // 10, 50, 38, 100

        std::vector<int>::iterator it;
        it = scores.begin();

        it = scores.erase(it);  // 50, 38, 100 
        ```
        - 재할당? 복사?
            - 앞에 것 하나 지우고 뒤에 하나씩 복사해 당겨온다.
            - 복사가 일어난다.
            - 즉, 재할당은 없고 복사는 있다.
    - 같은 자료형의 두 벡터 교환하기
        ```c++
        vector<int> scores;
        scores.reserve(2);

        scores.push_back(85);
        scores.push_back(73);  // 85, 73

        vector<int> anotherScores;
        anotherScores.assign(7, 100) // 100, 100, 100, 100, 100, 100, 100

        scores.swap(anotherScores);  // scores: 100, 100, 100, 100, 100, 100, 100
                                     // anotherScores: 85, 73
        ```
    - vector의 크기 변경
        - resize(size_t n);
        - 새 크기가 vector의 기존 크기보다 작은, 초과분이 제거됨
        - 새 크기가 vector의 용량보다 크면 재할당이 일어남
    - vector에서 모든 요소 제거 하기
        - clear();
        - vector를 싹 지운다.
        - 크기(size)는 0이 되고, 용량은 변하지 않는다.
    - 개체를 직접 보관하는 벡터
        ```c++
        // Score.h
        class Score
        {
        public:
            // ...

        pirvate:
            int mScore;
            string mClassName;
        };
        ```
        ```c++
        // Main.cpp
        vector<Score> scores;
        scores.reserve(4);

        scores.push_back(Score(30, "C++"));
        scores.push_back(Score(59, "Algorithm"));
        scores.push_back(Score(87, "Java"));
        scores.push_back(Score(41, "Andorid"));
        ```
        - 실제 socres안에는 개체의 포인터나 레퍼런스가 아닌
        - 실제 Score의 개체가 들어가 있는 것이다.
        - scores의 요소 하나는 개체(Score)의 사이즈 크기인 16바이트 메모리가 잡히게 된다.
        - 개체를 직접 보관하는 벡터의 문제점
            - 공간 부족으로 메모리 재할당이 일어난다. 클래스 전부 복사하게 된다.
            - 대입할 때 클래스 전부 복사 일어난다.
            - 이를 해결하기 위해 포인터를 저장
    - 포인터를 저장하는 벡터
        ```c++
        // Main.cpp
        vector<Score*> scores;
        scores.reserve(4);

        scores.push_back(new Score(30, "C++"));
        scores.push_back(new Score(87, "Java"));
        scores.push_back(new Score(41, "Andorid"));

        scores.clear();
        ```
        - 포인터를 저장하는 벡터의 재할당이 일어나는 단점도 있다.
    - 포인터 벡터 삭제하기
        ```c++
        vector<Score*> scores;
        scores.reserve(4);

        scores.push_back(new Score(30, "C++"));
        scores.push_back(new Score(87, "Java"));

        for (vector<Score*>::iterator iter = scores.begin(); iter != scores.end(); ++iter)
        {
            delete * iter;  // 모든 요소에 대해 delete를 꼭 호출할 것!!!
        }

        scores.clear();
        ```
    - 벡터의 장점
        - 순서에 상관 없이 요소에 임의적으로 접근 가능
        - 제일 마지막 위치에 요소를 빠르게 삽입 및 삭제
    - 벡터의 단점
        - 중간에 요소 삽입 및 삭제는 느림
        - 재할당 및 요소 복사에 드는 비용
    
