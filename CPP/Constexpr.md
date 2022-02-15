### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### constexpr
- #### constexpr의 의도
    - 컴파일 시에 값 평가를 강제하기 위해서 탬플릿 메타프로그래밍을 남용함
    - 이러지 않아도 컴파일러가 자발적으로 그렇게 해주는 경우도 있기 했음
    - constexpr가 프로그래머의 의도를 보여주는 더 나은 방법이다
    - 우리의 의도는 컴파일 도중에 값을 평가하는 것임을 컴파일러에게 알려줌
    - 컴파일러가 컴파일 도중에 변수들을 결정지어 줌
    - 컴파일 도중에 "반드시" 값이 결정되게 하려면 constexpr 변수를 쓸 것
    - 하지만 컴파일러가 거부할 수도 있다
        - 재귀함수 너무 많이 돌리게 되면 컴파일러가 거부할 수도 있다.

- #### 문자열 해쉬를 컴파일 도중에 만들 수 있다면?
    - 여전히 런타임 중에 실행되고 있는 문자열 해쉬
    - 컴파일 도중에 만들 수 있다면 
    - 문자열 비교에 드는 런타입 비용은 언제나 O(1)
    - **constexpr**을 써서 컴파일 타임에 문자열 해쉬를 만들 수 있게 됐다!
        ```c++
        constexpr unsigned long djb2_hash_impl(const char* text, unsigned long prev_hash)
        {
            return text[0] == '\0' ? prev_hash :
                djb2_hash_impl(&text[1], prev_hash * 33 ^ static_cast<unsigned long>(text[0]));
        }

        constexpr unsigned long djb2_hash(const char* text)
        {
            return djb2_hash_impl(text, 5381);
        }

        int main()
        {
            constexpr unsigned long coco = djb2_hash("Coco"); // mov  dword ptr [coco], 7C812CA5h
        }
        ```

- #### const vs constexpr
    - const : 변경을 불허
    - constexpr : 컴파일시에 평가해주면 좋겠음

- #### const 변수 vs constexpr 변수
    - 결국 둘다 const임

- #### cnost 함수 vs constexpr 함수
    - const 함수
        - 멤버 함수에만 사용 가능
        - 멤버 변수에는 바꿀 수 없음
    - constexpr 함수
        - 멤버와 비멤버 함수에 둘다 사용
        - 멤버 변수를 바꿀 수 있음

- #### 배열의 길이 정하기
    - enum
        ```c++
        // FixedArray.h
        class FixedArray
        {
        public:
            // ...
        private:
            enum { MAX = 10 };    // 선호
            int mSize;
            int mArray[MAX];     
        };
        ```
    - constexpr
        ```c++
        // FixedArray.h
        class FixedArray
        {
        public:
            // ...
        private:
            static constexpr int MAX = 10;
            int mSize;
            int mArray[MAX];
        };
        ```
