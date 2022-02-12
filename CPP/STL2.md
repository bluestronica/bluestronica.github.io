### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 표준 템플릿 라이브러리(STL, Standard Template Library) 2
- #### 맵(map)
    - 키(key)와 값(value)의 쌍들을 저장
    - 키는 중복될 수 없음
    - 이진 탐색 트리 기반으로하는 **자동 정렬되는 컨테이너**이다.
        - 순서없이 삽입한 키값들이 자동정렬되어 출력된다.
    - 장점
        - std::list나 std::vector보다 탐색 속도가 더 빠름
    - 단점
        - 자동으로 정렬됨
        - 그래서, 요소 삽입/제거가 빈번할 경우 성능이 저하됨
        - 해쉬맵이 아니다. 따라서 O(1)이 아님
    - O()
        - O(logN) : 어떤 것을 할 때 양쪽으로 두 개 나눠서 한쪽씩만 봐 내려간다.
            - 이진트리 기반의 맵은 O(logN)
        - O(N) : for문이 1부터 size까지 돈다.
            - std::list, std::vector
        - O(N^2) : 이중 for문이 돌아간다.
        - O(N^3) : 삼중 for문이 돌아간다.
    - map 만들기
        ```c++
        std::map<std::string, int> simpleScoreMap;
        std::map<StudentInfo, int> complexScoreMap;

        std::map<std::string, int> copiedSimpleScoremap(simpleScoreMap);
        ```
    - 두 데이터를 한 단위로 저장하는 구조
        - std::pair<key, value>
    - 새 요소를 map에 삽입
        - std::pair<iterator, bool> insert(const value_type& value)
            - 반복자와 bool 값을 한 쌍으로 반환한다.
            - 반복자는 요소를 가리키고
            - bool값은 삽입 결과를 알려준다.
        - 키를 중복으로 삽입할 수 없다.
            ```c++
            // <iterator, true>를 반환한다
            simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));

            // <iterator, false>를 반환한다.
            simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 0));
            ```
    - operator[]
        - mppped_type& operator[](const key& key);
        - key에 대응하는 값을 참조로 반환한다.
        - map에 키가 없으면 새 요소를 삽입
        - map에 키가 이미 있으면 그 값을 덮어 씀
            - 키값으로 "Coco" 있을거라 생각하고 
            - return map["Coco"];를 리턴해버리면
            - "Coco" 키값으로 값ㅇ르 0으로 초기화해서 삽입이 되어버린다.
            - 의도하지 않는 값이 삽업되어 위험하다. 주의해한다.
        ```c++
        std::map<std::string, int> simpleScoreMap;

        simpleScoreMap["Coco"] = 10;   // 새 요소를 삽입한다.
        simpleScoreMap["Coco"] = 50;   // "Coco"의 값을 덮어쓴다.
        ```
    - 자동 정렬
        ```c++
        simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));
        simpleScoreMap.insert(std::pair<std::string, int>("Coco", 50));

        for (std::map<std::string, int>::iterator it = simpleScoreMap.begin(); it != simpleScoreMap.end(); ++i)
        {
            std::out << "(" << it->first << ", " << it->second << ")" std::endl;

            // ("Coco", 50)
            // ("Mocha", 100)
            // 삽입을 모카, 코코로 삽입을 했지만
            // 출력은 키값으로 자동정렬되어 나온다.
        }
        ```
    - 요소 찾기
        - iterator find(const key& key);
        - map에서 key를 찾으면 그에 대응하는 값을 가리키는 반복자를 반환한다.
        - 못 찾으면 end()를 반환
            ```c++
            std::map<std::string, int> simpleScoreMap;
            simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));

            std::map<std::string, int>::iterator it = simpleScoreMap.find("Mocha");
            if (it != simpleScorMap.end())
            {
                it->second = 80;
            }
            ```
    - 두 map을 스왑
        - void swap(map& other);
    - map을 비운다
        - void clear();
        ```c++
        std::map<std::string, int> ScoreMap;
        std::map<std::string, int> anotherScoreMap;

        scoreMap.swap(anotherScoreMap);

        anotherScoreMap.clear();  // anotherScoreMap empty
        ```
    - map의 요소들을 제거
        ```c++
        std::map<std::string, int>::iterator foundIt = simpleScoreMap.find("Coco");
        simpleScoreMap.erase(foundIt);

        simpleScoreMap.erase("Coco");
        ```
    - 개체를 키로 사용하기
        ```c++
        // StudentInfo.h
        class StudenInfo
        {
        public:
            // ...
        private:
            std::string mName;
            std::string mStudentId;
        };        
        ```
        ```c++
        // Main.cpp
        std::map<StudendInfo, int> socres;
        scores.insert(std::pair<StudentInfo, int>(StudentInfo("Poppy", "a334"), 30));
        scores.insert(std::pair<StudentInfo, int>(StudentInfo("Lulu", "a123"), 50));

        // '<' 비교하는 연산자를 만들어야 한다.
        ```
        - 왜 비교 연산이 필요할까?
            - STL map은 항상 정렬된다.
            - 그래서 반드시 두 키를 비교하는 함수를 만들어야 한다.
        - Operator<()
            ```c++
            bool StudentInfo::operator<(const StudentInfo& other) const
            {
                if (mName == other.mName)
                {
                    return mStudentID < other.mStudentID;
                }

                return mName < other.mName;
            }
            ```
        - 클래스 안에 함수로 만들어야 STL map에서 사용할 수가 있다.
        - 요소들을 정렬하는 또 다른 방법
            - map을 만들 때 비교함수(comparer)를 넣을 수도 있다.
            - 남이 만들어 놓은 구조체에 접근이 불가능할 때, 바꿀 권한이 없을 때 사용
                ```c++
                struct StudentInfoComparer
                {
                    bool operator()(const StudentInfo& left, const StudentInfo& right) const
                    {
                        return (left.getName() < right.getname());
                    }
                };

                // Main.cpp
                std::map<StudentInfo, int, StudentInfoComparer> Scores;
                ```


- ### 셋(Set)
    - 셋은 맵과 거의 같다.
    - 맵은 키와 값이 들어가 있고
    - 셋은 키만 들어가 있다. 결국 키는 값하고 같은 것이다.
    - 셋 만들기
        ```c++
        std::set<int> scores;
        scores.insert(20);
        scores.insert(100);

        for (std::set<int>::iterator it = scores.begin(); it != scores.end(); ++it)
        {
            std::cout << *it << std::endl;  // 20\n 100\n
        }
        ```
    -장점과 단점
        - 맵과 같음
        
- ### unordered_map
    - 키와 값으 쌍들을 저장
    - 키는 중복 불가
    - 자동으로 정렬되지 않는 컨테이너
        - 요소는 해쉬함수가 생성한느 색인(index)기반의 버킷(bucket)들로 구성됨
        - 해쉬맵이라고도 한다.
    - 요소 삽입하기
        - score["Coco"] = 100
        - 키["Coco"]-->해쉬함수-->인덱스[4]
        - 버킷[0 | 1 | 2 | 3 | 4(100) | 5] : 인덱스 4에 값 100이 들어있다.
    - 해쉬 충돌
        - 가끔 키가 다른데도 같은 해쉬 값이 나옴
        - 이럴 경우, 하나의 버킷에 둘 이상의 데이터 들어감
    - 버킷 내용 보여주기
        ```c++
        std::unordered_map<std::string, int> scores;

        scores["Nana"] = 60;
        scores["Mocha"] = 70;
        scores["Coco"] = 100;
        scores["Ari"] = 50;
        scores["Chris"] = 90;

        for (size_t i = 0; i < scores.bucket_count(); ++i)
        {
            std::cout << "Bucket #" << i << ": ";
            for (auto it = scores.begin(i); it != scores.end(i); ++it)
            {
                std::cout << " " it->first << ":" << it->second;
            }
            std::cout << std::endl;
        }
        ```

- ### unordered_set
    ```c++
        std::unordered_set<std::string> names;
        scores.insert("Victor");
        scores.insert("Lulu");
        scores.insert("Mocha");        

        for (std::set<int>::iterator it = scores.begin(); it != scores.end(); ++it)
        {
            std::cout << *it << std::endl;  // Victor\n Mocha\n  Lulu\n
        }
        ```
    
- ### array
    - 요소 수를 기억하지 않는 C스타일 배열을 추상화한 것
    - 그래서 이게 필요한 이유에 대해 100%확신은 못 하겠음
        ```c++
        // FixedVector
        FixedVector<int, 3> numbers;

        numbers.Add(1);
        std::cout << numbers.GetSize() << std::endl;      // 1
        std::cout << numbers.GetCapacity() << std::endl;  // 3
        ```
        ```c++
        // std::arry
        std::arry<int, 3> numbers;

        numbers[0] = 1;
        std::cout << numbers.size() << std::endl;      // 3
        std::cout << numbers.max_size() << std::endl;  // 3
        ```

- 범위 기반 for 반복문
    - for 반복문을 더 간단하게 쓸 수 있음
    - 이전의 for 반복보다 가독성이 더 높음
    - STL 컨테이너와 C스타일 배열 모두에서 작동한다.
    - auto 키워드를 범위 기반 for 반복에 쓸 수 있음
    - 컨테이너/배열을 역순회할 수 없음
    - for_each()
        - C++03
        - 범위 기반 for만큼 가독성이 높지 않음
        - 좀 이상함
        - 그러니, 범위 기반 for를 쓰자!!
    - 값으로, 그리고 참조로
        ```c++
        // {["Lulu", 10], ["Chris", 50], ["Monica", 100]}
        std::map<std::string, int> scoreMap;

        for (auto score : scoreMap)
        {
            score.second -= 10;
            std::cout << score.first << ": " << score.second << std::endl;

            // Chris:40
            // Lulu:0
            // Monica:90
            // 값을 복사했기 때문에 복사된 값이 변경된 것이다
            // 원본의 값은 변하지 않았다.
        }

        for (auto& score : scoreMap)
        {
            std::cout << score.first << ": " << score.second << std::endl;

            // Chris:50
            // Lulu:10
            // Monica:100
            // 원본의 값은 변하지 않았다.
            // 원본을 바꾸고 싶으면 잠조자를 사용하라!!
        }
        ```
    - 참조로, 그리고 const
        ```c++
        // {["Lulu", 10], ["Chris", 50], ["Monica", 100]}
        std::map<std::string, int> scores;

        for (auto& score : scores)
        {
            score.second -= 10;
            std::cout << score.first << ": " << score.second << std::endl;

            // Chris:40
            // Lulu:0
            // Monica:90
            // 참조변수로 받았기 때문에 원본 값이 변경되었다.
        }

        for (const auto& score : scores)
        {
            std::cout << score.first << ": " << score.second << std::endl;

            // 참조변수로 받았기 때문에 원본 값이 변경되어있다.
            // Chris:40
            // Lulu:0
            // Monica:90
            // 참조변수로 받더라도 수정을 하고 싶지 않을 때 const를 써라!
        }
        ```