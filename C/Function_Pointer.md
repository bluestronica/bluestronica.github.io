[bluestronica.github.io/C](https://bluestronica.github.io/C)

# 함수 포인터
### 함수도 어디다 저장해 둔 뒤 매개변수로 잔달할 수 없을까?
```c
result = calculate(op1, op2, operator);  // operator = add()
result = calculate(op1, op2, operator);  // operator = sub()
result = calculate(op1, op2, operator);  // operator = mul()
result = calculate(op1, op2, operator);  // operator = div()
```
