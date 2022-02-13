### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 스마트(Smart) 포인터 1
- #### 포인터
    ```c++
    int main()
    {
        Vector* myVector = new Vector(10.f, 30.f);

        // ...

        delete myVector;

        return 0;
    }
    ```
    - 더 이상 포인터가 필요하지 않을 때 메모리를 해제해야 한다.
        - 가리키는 주소의 데이타를 해제 시켜야 메모리 누수가 없어진다.
        - 강제로 **delete**를 직접 호출해서 해제시켜야한다.
    - **스마트 포인터**를 쓰면 메모리 해제를 위한 **delete**를 직접 호출할 필요가 없다.
    - 그리고 가비지 컬렉션보다 빠르다!

- #### 유니크 포인터(unique_ptr)