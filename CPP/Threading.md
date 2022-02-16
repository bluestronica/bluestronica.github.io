### [bluestronica.github.io/CPP](https://bluestronica.github.io/CPP)

### 쓰레딩(Threading) 라이브러리
- #### 쓰레딩(Threading) 지원 라이브러리
    - 쓰레딩 라이브러리 사용법에 초점을 맞춤
        - 쓰레드
        - 뮤텍스(mutex)
        - 조건 변수(condition_variables)
        - 더 있음.
    - C++11 전까지 표준 멀티쓰레딩 라이브러리가 없었음
    - 그래서 OS마다 멀티쓰레딩 구현이 달랐음
        - 리눅스/유닉스 : POSIX 쓰레드(pthreads)
        - 윈도우 쓰레드

- #### std::thread
    - 표준 c++쓰레드
        ```c++
        #include <iostream>
        #include <string>
        #include <thread>

        void PrintMessage(const std::string& message)
        {
            std::cout <<message << std::endl;
        }

        int main()
        {
            std::thread thread(PrintMessage, "Message from a child thred.");

            PrintMessage("Message from a main thread.");

            return 0;


            // message 두 번 연속 출력되고 뉴 라인 두 번 연속 출력된다.
            // 이러한 현상을 멀티스레드의 레이싱컨디션이라 한다.
        }
        ```
    - 이동(move) 가능
        ```c++
        std::thread PrintThread(PrintMessage, 10);

        std::thread movedThread(std::move(printThread));  
        ```
    - 복사 불가능
        ```c++
        std::thread PrintThread(PrintMessage, 10);

        std::thread movedThread(std::move(printThread));
        std::thread copiedThread = movedThread;           // 컴파일 에러 , 복사 불가능 
        ```
    - **`std::thread::join()`**
        - 쓰레드 개체가 끝날 때까지 현재 쓰레드를 먼춰 놓는다.
        - 이 함수를 호출한 후 쓰레드 개체를 안전하게 소멸시킬수 있음
            ```c++
            std::thread thread(PrintMessage);
            thread.join();
            ```
    - std::thread::get_id()
        - 쓰레드 ID를 반환한다.
            ```c++
            #include <iostream>
            #include <string>
            #include <thread>

            void PrintMessage(const std::string& message)
            {
                std::cout <<message << std::endl;
            }

            int main()
            {
                std::thread thread(PrintMessage, "Message from a child thred.");

                std::thread::id childThreadID = thread.get_id();
                std::stringstream ss;
                ss << childThreadID;
                std::string childThreadIDStr = ss.str();

                PrintMessage("waiting the child thread(ID: " + childThreadIDStr + ")");

                thread.join();

                return 0;
            }
            ```
    - std::thread::detach()
        - 쓰레드 개체에서 쓰레드를 떼어 낸다.
        - 떼어진 쓰레드는 메인 쓰레드와 무관하게 독립적으로 실행됨
            ```c++
            #include <iostream>
            #include <string>
            #include <thread>

            void PrintMessage(const std::string& message, int count)
            {
                for (int i = 0; i < count; ++i)
                {
                    std::cout << i + 1 << ": " << message << std::endl;
                }
            }

            int main()
            {
                std::thread thread(PrintMessage, "Message from a child thred.", 10);

                PrintMessage("waiting the child thread...", 1);

                thread.detach();

                return 0;
            }
            ```
    - std::thread::joinable()
        - 쓰레드가 실행 중인 활성 쓰레드인지 아닌지 확인한다.
            ```c++
            #include <iostream>
            #include <string>
            #include <thread>

            void PrintMessage(const std::string& message, int count)
            {
                for (int i = 0; i < count; ++i)
                {
                    std::cout << i + 1 << ": " << message << std::endl;
                }
            }

            int main()
            {
                std::thread thread(PrintMessage, "Message from a child thred.", 10);

                PrintMessage("waiting the child thread...", 1);

                thread.detach();
                // ...
                if (thrad.joinable())
                {
                    thread.join();
                }

                return 0;
            }
            ```
    - std::ref()
        - T의 참조를 내포한 reference_wrapper 개체를 반환한다.
            - 매개변수를 참조로 전달
            ```c++
            #include <iostream>
            #include <string>
            #include <thread>

            int main()
            {
                std::vector<int> list(100, 1);
                int result = 0;

                std::thread thread([](const std::vector<int>& v, int& result)
                {
                    for (auto item : v)
                    {
                        return += item;
                    }                    
                }, list, std::ref(result);

                thread.join();

                std::cout << "Result: " << resutl << std::endl;

                return 0;
            }
            ```

- #### std::this_thread
    - 네임스페이스 그룹
    - 현재 쓰레드에 적용되는 도우미 함수들이 있음
        - get_id()
        - sleep_for()
            - 최소한 sleep_duration 만큼의 시간 동안 현재 쓰레드의 실행을 멈춘다.
                ```c++
                std::this_thread::sleep_for(std::chrono::seconds(1));
                ```
        - sleep_until()
        - yield()
            - sleep과 비슷하나, sleep이 쓰레드를 일시정지 상태로 바꾸는 반면
            - yield는 계속 실행 대기 상태
            - 잠들고 일어나는 순간에 오버헤드 및 느려지는 경우가 있기에
            - yield를 쓰는게 더 낫다.

- #### std::mutex
    - std::mutex::lock()
        - 뮤텍스를 잠근다
        - 동일한 쓰레드에서 두 번 잠그면 데드락(deadlock) 발생
            - 꼭 그렇게 해야 된다면, std::recursive_mutex를 사용
    - std::mutex::unlock()
        - 뮤텍스 잠금을 푼다.
        - 현재 쓰레드에서 잠긴 적이 없을 때의 행동은 정의되지 않음

- #### std::scoped_lock
    - 매개변수로 전달된 뮤텍스(들)을 내포하는 개체를 만듬
    - 개체 생성시에 뮤텍스를 잠그고 범위(scope)를 벗어나 소멸될 때 품
    - 데드락을 방지
        ```c++
        #include <iostream>
        #include <mutex>
        #include <string>
        #include <thread>

        void PrintMessage(const std::string& message)
        {
            static std::mutex sMutex;

            {  // lock은 최소 범위로 지정해서
                std::scoped_lock<std::mutex> lock(sMutex);
                std::cout << "Messae from thread ID " << std::this_thread::get_id() << std::endl;
            }

            {  // lock은 최소 범위로 지정해서
                std::scoped_lock<std::mutex> lock(sMutex);
                std::cout <<message << std::endl;
            }
        }

        int main()
        {
            std::thread thread(PrintMessage, "Message from a child thred.");

            PrintMessage("Message from a main thread.");

            thread.join();

            return 0;
        }
        ```

- #### std::condition_variable
    - 이벤트 개체
    - 신호를 받을 때까지 현재 쓰레드의 실행을 멈춤
    - notify_one(), notify_all()
        - 멈춰 놓은 쓰레드 하나 또는 전부를 다시 실행시킴
    - wait(), wait_for(), wait_until()
        - 조건 변수의 조건을 충족시킬때까지 또는 일정 시간 동안 현재 쓰레드의 실행을 멈춤    
    - std::unique_lock을 사용해야 함
    - https:://en.cppreference.com/w/cpp/thread/condition_variable

- #### std::unique_lock
    - 기본적으로 scoped lock
    - 생성시에 lock을 잠그지 않을 수도 있음 (두 번째 매개변수로 std::defer_lock을 전달할 것)
    - std::recursive_mutex와 함께 써서 재귀적으로 잠글 수 있음
    - 조건 변수에 쓸 수 있는 유일한 lock