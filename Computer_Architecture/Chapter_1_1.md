```C
#include <stdio.h>
#include <stdbool.h> // vs 컴파일 true

#define LENGTH (4094)

int main(void)
{
    char line[LENGTH];
    char real_number[LENGTH];
    
    printf("ieee754_32bit_floating_point_number \n");

    while (true) // vs : true
    {
        // 아무것도 입력하지 않고 엔터 쳤을 때 반응
        if (fgets(line, LENGTH, stdin) == NULL)  // vs : NULL
        {
            clearerr(stdin); // 오류 표시자 지워주는 함수
            break;
        }

        // vs 컴파일은 sscanf => sscanf_s
        if (sscanf_s(line, "%s", real_number, sizeof(real_number)) == 1)
        {
            printf("실수 : %s\n", real_number);
        }
    }


    return 0;
}
```
