### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### STL 알고리듬(Algorithm)
- #### STL 알고리듬이란?
    - 요소 범위에서 쓸 수 있는 함수들
        - [처음, 마지막]
    - 배열 또는 몇몇 STL 컨테이너에 쓸 수 있음
    - 반복자를 통해 컨테이너에 접근
    - 컨테이너의 크기를 변경하지 않음
    - 따라서 추가 메모리 할동도 없음

- #### STL 알고리듬의 유형
    - 변경 불가 순차(sequence) 연사
        - fine(), for_each(), ...
    - 변경 가능 순차 연산
        - copy(), swap(), ...
    - 정렬 관련 연산
        - sort(), merge(), ...
    - 범용 수치 연산
        - accumulate(), ...

- #### vector를 다른 vector로 복사하기
    ```c++
    #include <algorithm>
    // ...

    int main()
    {
        std::vector<int> scores;

        scores.push_back(10);
        scores.push_back(20);
        scores.push_back(30);

        std::vector<int> copiedScores;
        copiedScores.resize(scores.size());

        std::copy(scores.begin(), scores.end(), copiedScores.begin());

        for (auto it = copiedScores.begin(); it != copiedScores.end(); ++it)
        {
            std::cout << *it << std::endl;
        }

        return 0;
    }
    ```
    - copy() 구현
        ```c++
        template<class _InIt, class _OutIt>
        _OutIt copy(_IntIt _First, _InIt _Last, _OutIt _Dest)
        {
            for (; _First != _Last; ++_Dest, (void)++_First)
            {
                *_Dest = *_First;
            }

            return (_Dest);
        }
        ```

- #### STL 알고리듬 목록
    - 너무 많음
    - 여기 참고: http://www.cplusplus.com/reference/algorithm/