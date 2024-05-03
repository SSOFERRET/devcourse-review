:one: break & continue

1. break: 반복문을 중간에 빠져나가게끔 하는 구문.
2. continue: skip하고 다시 반복문의 반복 조건으로 올라간다.

❔ **평가 문제** 구구단을 출력하되, 짝수단(2단, 4단, 6단, 8단)만 출력하는 프로그램을 작성하라. 단, continue문을 사용하여 작성할 것.
```c
#include <stdio.h>

void main()
{
    for (int i = 2; i < 10; i++) {
        for (int j = 2; j < 10; j++) {
            if (j % 2 == 1) continue;
            printf("%d*%d = %d\n", i, j, i*j);
        }
    }
}
```

---

:two: 함수

- input에 따른 output이 나오는 상자
- 코드에서 함수를 사용하는 목적
  - 코드를 나누어서 처리하기 위함: 가독성 향상, 유지 보수 및 확장 용이
- 함수의 종류
  - 표준 함수: 언어에서 기본적으로 제공해주는 함수. ex) printf()
  - 사용자 정의 함수: 사용자가 자신이 원하는 기능을 직접 만들어 사용하는 함수.
- 함수의 기본 형태
```
자료형 함수이름(인수 목록) {
  함수의 내용
}
```
- void 타입: 결과값을 리턴하지 않는 함수.

❔ **평가 문제** 사각형의 넓이를 구하는 함수를 작성해보자. 그리고 이 함수를 main에서 호출하여 출력해보자. 
```c
int square(int width, int height) {
  return width*height;
} 

void main() {
    printf("square: %d", square(5, 10));
}
```

❔ **평가 문제** 사용자로부터 두 수를 입력 받아 두 수를 비교하여 최대값과 최소값을 구하는 함수를 정의하라. 그리고 이 함수를 호출하여 결과값을 출력하도록 하라.
```c
#include <stdio.h>

int getMin(int a, int b) {
  if (a >= b) return b;
  return a;
} 

int getMax(int a, int b) {
  if (a <= b) return b;
  return a;
}

void main()
{   
    int a = 5;
    int b = 3;
    printf("min: %d\n", getMin(a, b));
    printf("max: %d\n", getMax(a, b));
}
```

❔ **평가 문제** 커피 자판기가 있다. 100원을 넣으면 블랙커피, 200원을 넣으면 밀크커피가 나온다. 자판기를 함수로 구현해보자.
```c
#include <stdio.h>

void getCoffee(int price) {
    char output[10];
    if (price == 100) 
        printf("블랙 커피");
    else if (price == 200)
        printf("밀크 커피");
    else
        printf("잘못된 입력");
}

void main()
{   
    getCoffee(100);
    printf("\n");
    getCoffee(200);
    printf("\n");
    getCoffee(150);
}
```
---

