### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 람다 식(Lambda Expression)
- #### 람다 식이란?
    - 이름이 없는 함수 개체
    - 내포(nested)되는 함수
        ```c++
        [<cpatures>](<parameters>)<specifiers> -> <return_type>
        {
            <body>
        }
        [캡처 블록](매개변수 목록) <지정자> -> <반환 형>
        {
            함수 바디
        }
        ```
    - ##### 캡처 블록
        - 람다 식을 품는 범위(scope)안에 있는 **변수**를 **람다 식에 넘겨줄 때** 사용
        - [] : 캡처하지 않는 경우
            ```c++
            int main()
            {
                // 방법 1
                auto noCapture = []() { std::cout << "No capturing example 1"<< std::endl; };
                noCapture();

                // 방법 2. 바로 실행
                [] { std::cout << "No capturing example 2"<< std::endl; }();
                // ...
            }
            ```
        - [] : (에러) 외부 변수 사용하기
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto max = []() { return score1 > score2 ? score1 : score2; }; // 컴파일 에러

                std::cout << "Max value is " << max() << std::endl;
                // ...
            }
            ```
        - [=] : 값에 의한 캡처
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto max = [=]() { return score1 > score2 ? score1 : score2; }; 

                std::cout << "Max value is " << max() << std::endl;
                // ...
            }
            ```
        - [=] : (에러) 값에 의한 캡처
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto changeValue = [=]()
                {
                    score1 = 100.f;       // 컴파일 에러, score1을 수정할 수 없음
                };                

                changeValue();
                // ...
            }
            ```
        - [&] : 참조에 의한 캡처
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto changeValue = [&]()
                {
                    score1 = 100.f;
                    score2 = 55.4f
                };                

                changeValue();  // 함수가 실행 될 때 score1, score2 값이 바뀐다.
                // ...
            }
            ```
        - [score1, score3] : 값에 의한 특정 변수 캡처
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;
                float score3 = 100.f;

                auto printValue = [score1, score3]()
                {
                    // score1, score3에는 접근, 하지만 score2에 접근할 수 없음
                    std::cout << socre1 << " / " << score3 << std::endl;
                };

                printValue();
                // ...
            }
            ```
        - [&score1] : 참조에 의한 특정 변수 캡처
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto changeValue = [&score1]()
                {
                    score1 = 100.f;
                    // 여기서 score2에 접근할 수 없음
                };

                changeValue();
                // ...
            }
            ```
        - [=, &score1] : 캡처 옵션 섞기
            ```c++
            int main()
            {
                float score1 = 80.f;
                float score2 = 20.f;

                auto changeValue = [=, &score1]()
                {
                    score1 = 100.f;       // 참조에 의한 캡처    
                    std::cout << score2   // 값에 의한 캡처

                    // &score1만 참조로 캡쳐하고
                    // =은 나머지 전부다 값으로 캡쳐
                    // 참조로 캡쳐한 score1은 값 변경이 가능하고
                    // 값으로 캡쳐한 나머지는 값 변경 못한다.
                };

                changeValue();
                // ...
            }
            ```
    - ##### 매개변수 목록
        - 선택 사항
        - 빈 괄호를 생략할 수 있음
        - 하지만 빈 괄호 생락은 하지말자. 헷갈린다.
        - 예시 : 두 값 더하기(이런 모양은 별로!)
            ```c++
            int main()
            {
                int score1 = 70;
                int score2 = 85;

                auto add = [](int a, int b)
                {
                    return a + b;
                };

                std::cout << add(score1, score2) << std::endl;
                // ...
            }
            ```
        - 예시 : 정렬하기
            ```c++
            #include <algorithm>
            #include <vector>

            int main()
            {
                std::vector<float> socres;

                scores.push_back(50.f);
                scores.push_back(88.5f);
                scores.push_back(70.f);

                std::sort(scores.begin(), scores.end(), [](float a, floatb) { return (a > b); });
                std::sort(scores.begin(), scores.end(), [](float a, floatb) { return (a <=> b); });

                // 정렬하기는 람다가 좋다.
                // 일회용 사용이기때문에
            }
            ```
    - ##### 지정자
        - 선택 사항
        - mutable
            - 값에 의해 캡쳐된 개체를 수정할 수 있게 함
            - 괜찮은 언어 디자인, 허나 c++에 너무 늦게 들어옴
                ```c++
                int main()
                {
                    int value = 100;

                    auto foo = [value]() mutable
                    {
                        std::cout << ++value << std::endl;
                        // 원본을 바꾸는게 아니 사본을 바꾼다.
                        // foo의 vlaue 값이 101이 되고
                        // main의 value의 값은 여전히 100이다.
                    };

                    foo();
                    // ...
                }
                ``
    - ##### 반환 형
        - 선택 사항
        - 반환 형을 적지 않으면 반환문을 통해 유추해 줌
        - 자동 반환 형식 :(

- #### 람다식의 장단점
    - 장점 
        - 간단한 함수를 빠르게 작성할 수 있음
    - 단점
        - 디버깅하기 힘들어짐
        - 함수 재사용성이 낮음
        - 사람들은 보통 함수를 새로 만들기 전에 클래스에 있는 기존 함수를 찾아 봄
        - 람다 함수는 눈에 잘 띄지 앟아서 못 찾을 가능성이 높음
        - 그럼 코드 중복이 발생

- #### 베스트 프랙티스
    - 기본적으로, 이름 있는 함수를 쓰자(전통적인 방식)
    - 자잘한 함수는 람다로 써도 괜찮음(한 줄짜리 함수)
    - 허나 재사용할 수 있는 함수를 찾기 좀 어려움
    - 정령 함수 처럼 STL 컨테이너에 매개변수로 전달할 함수들도 좋은 후보
