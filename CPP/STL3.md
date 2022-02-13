### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 표준 템플릿 라이브러리(STL, Standard Template Library) 3
- #### 큐(queue)
    - 선입 선출(First-in first-out, FIFO) 자료구조
        ```c++
        #include <queue>

        int main()
        {
            std::queue<std::string> studentNameQueue;
            sutdentNameQueue.push("Coco");
            sutdentNameQueue.push("Mocha");

            while (!studentNameQueue.empty())
            {
                std::cout << "Waiting student: " << studentNameQueue.front() << std::endl;
                studentNameQueue.pop();
            }

            return 0;
        }
        ```
    - 가장 먼저 삽입되었던 요소를 참조로 반환
        - value_type& front();
    - 가장 마지막에 삽입되었던 요소를 참조로 반환
        - value_type& backe();
    - queue에 들어 있는 요소의 수를 반환
        - size_type size();
    - queue가 비어 있으면 true, 그렇지 않으면 false 반환
        - bool empty();
    - size()보다 empty()를 더 많이 사용한다.

- #### 스택(stac)
    - 후입 선출(Last-in first-out, LIFO) 자료구조
        ```c++
        #include <stack>

        int main()
        {
            std::stack<std::string> studentNameStack;
            sutdentNameStack.push("Coco");
            sutdentNameStack.push("Mocha");

            while (!studentNameStack.empty())
            {
                std::cout << studentNameStack.top() << std::endl;
                studentNameStack.pop();
            }

            return 0;
        }
        ```
    - 맨위의 요소를 읽어 오는 행위
        - value_type& top();
    - 맨위의 요소를 제거하는 행위
        - pop()
    - 요쇼의 수를 가져오기
        - size_type size();
    - stack이 비어 있으면 true, 그렇지 않으면 false를 반환
        - bool empty();

- #### 리스트(list)
    ```c++
    #include <list>

    int main()
    {
        std::list<int> scores;
        scores.push_front(10);  // 10
        scores.push_front(20);  // 20, 10
        scores.push_front(30);  // 30, 20, 10

        return 0;
    }
    ```
    - 양뱡향 연결 리스트(이중 연결 리스트)
    - list보다 vector가 더 빠른 경우가 많은 것은
    - 메모리 불연속적인(cpu 캐쉬와 잘 동작하지 못함) 이유이기 때문이다.
    - 그래서 리스트를 많이 사용하지 않는다.

- #### STL 컨테이너가 더 있다
    - 멀티셋(multi-set)
        - 중복 키를 허용
        - 요소를 수정하면 안 됨
    - 멀티맵(multi-map)
        - 중복 키를 허용
    - 덱(디큐, deque)
        - Double-ended queue의 약자
        - 양쪽 끝에서 요소 삽입과 삭제가 모두 가능
    - 우선순위 큐(priority-queue)
        - 자동 정렬되는 큐

- #### 회사내부에서 만드는 STL 대체품들...
    - 많은 회사들이 자신마의 컨테이너를 만들어 STL을 대체
        - EA
            - EASTL
            - STL과 호환 됨
            - 메모리 문제 등을 고친 컨테이너
        - Epic Games (언리얼 엔진 4)
            - TArray, TMap, TMultiMap, TSet
            - STL보다 나은 인터페이스로 구연한 언리얼만의 컨테이너
    - EA STL과 언리얼 엔진 4는 모두 오픈 소스
    - 직접 컨테이너를 만들어 보면 메모리 관리에 대한 이해를 높일 수 있다. 