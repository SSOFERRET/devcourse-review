## :one: break & continue

1. break: 반복문을 중간에 빠져나가게끔 하는 구문.
2. continue: skip하고 다시 반복문의 반복 조건으로 올라간다.

❔ **평가 문제 >>** 구구단을 출력하되, 짝수단(2단, 4단, 6단, 8단)만 출력하는 프로그램을 작성하라. 단, continue문을 사용하여 작성할 것.
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

## :two: 함수

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

❔ **평가 문제 >>** 사각형의 넓이를 구하는 함수를 작성해보자. 그리고 이 함수를 main에서 호출하여 출력해보자. 
```c
int square(int width, int height) {
  return width*height;
} 

void main() {
    printf("square: %d", square(5, 10));
}
```

❔ **평가 문제 >>** 사용자로부터 두 수를 입력 받아 두 수를 비교하여 최대값과 최소값을 구하는 함수를 정의하라. 그리고 이 함수를 호출하여 결과값을 출력하도록 하라.
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

❔ **평가 문제 >>** 커피 자판기가 있다. 100원을 넣으면 블랙커피, 200원을 넣으면 밀크커피가 나온다. 자판기를 함수로 구현해보자.
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

## :three: 변수의 범위

- 지역 변수
  - 지역 변수는 스택 메모리에 쌓인다.(FILO)
  - 변수 이름은 같아도 다른 영역에 속한 변수는 독립된 다른 변수이다.
  - 지역 변수는 속한 함수의 수명이 끝날 때 소멸된다.
  - 함수 호출 순서와 소멸 순서는 반대이다.
- 전역 변수
  - 어디서든 접근할 수 있는 변수.
  - 전역 변수는 프로그램이 종료될 때 소멸된다.
  - 데이터 영역에 저장된다.
- static 변수
  - 지역 변수처럼 중괄호 영역에서 선언되지만, 중괄호를 벗어나도 메모리 상에 고정되어 소멸하지 않는다.
  - 함수 호출 시 생성되고 프로그램 끝날 때 소멸된다.

❔ **평가 문제 >>**  내가 읽은 책들의 페이지 수를 누적 게산하는 기능을 가진 책읽기 마라톤 기능을 가진 프로그램 구현을 한다. 함수로 구현하되, 페이지의 누적 결과를 저장하는 변수를 전역 변수로도 구현하고, statkc 변수로도 구현해보도록 한다.
```
//전역 변수
#include <stdio.h>

int totalPages = 0;

int getTotalPages(int pages) {
    totalPages += pages;
    return totalPages;
}

void printPages() {
    int todayPages;
    
    
    printf("읽은 책의 페이지 수를 입력하시오: ");
    scanf("%d", &todayPages);
    
    if (todayPages == -1) 
        printf("더 분발하세요.");
    else {
        printf("최종 누적 페이지: %d\n", getTotalPages(todayPages));
        printPages();
    }
}

void main()
{   
    printPages();
}
```

```
//static 변수
#include <stdio.h>

int getTotalPages(int pages) {
    static int totalPages = 0;
    totalPages += pages;
    return totalPages;
}

void printPages() {
    int todayPages;
    
    
    printf("읽은 책의 페이지 수를 입력하시오: ");
    scanf("%d", &todayPages);
    
    if (todayPages == -1) 
        printf("더 분발하세요.");
    else {
        printf("최종 누적 페이지: %d\n", getTotalPages(todayPages));
        printPages();
    }
}

void main()
{   
    printPages();
}
```

---

## :four: 배열

- 같은 용도, 같은 타입의 수많은 변수를 한꺼번에 만들기 위해 사용한다.
- 배열의 선언 구조
```
int array[5];
// int 타입의 데이터 5개를 요소로 삼는 배열 선언.

int array2[] = {1, 2, 3, 4, 5};
// 배열의 초기화.
```
- 배열의 초기화: 배열의 길이 생략. 초기값의 개수를 보고 컴파일러는 배열의 길이를 계산한다.
- 배열의 복사: 배열은 요소끼리 복사해야한다.

❔ **평가 문제 >>**  배열 arr1의 값을 배열 arr2에 복사하되, 배열 요소를 역순으로 저장하도록하고, 복사된 arr2의 요소값들을 출력하도록 하라.
```
#include <stdio.h>

int main()
{   
    int i;
    int arr1[5] = {1, 2, 3, 4, 5};
    int arr2[5];
    
    for (int i = 0; i < 5; i++) {
        arr2[4 - i] = arr1[i];
    }
    
    for (int i = 0; i < 5; i++) {
        printf("%d", arr2[i]);
    }
}
```

- 문자열 변수: 문자열에 이름을 붙여주면 변수로 사용 가능하다.
```
char str[12] = "Hello World!";
//null값이 들어갈 마지막 공간까지 고려해서 배열 길이를 정해야한다.
```
- 문자열 변수에서 null 문자가 왜 필요한가?
  - 컴퓨터가 문자열의 끝을 인식하기 위해 null을 표시.

---

## :five: 포인터

- 메모리는 주소를 통해 메모리에 접근하여 값을 읽고 쓸 수 있다.
- 포인터는 포인터 변수의 줄임말로, 메모리의 주소값을 저장하고 있는 변수이다.
- 포인터 사용 방법
  - 일반 변수명 앞에 * 기호 붙여주기. 주소값만 저장하겠다는 뜻이다.
  - & 기호를 통해 변수의 주소값을 얻어갈 수 있다.
```c
#include <stdio.h>

int main()
{
    int b = 100;
    int *pB = &b; // 변수 b의 주소를 받아옴
    
    printf("b = %d\n", b);
    printf("&b = %p\n", &b);
    printf("pB = %p\n", pB);
    
    return 0;
}
/**
* b = 100
* &b = 0x7ffc6d94d50c
* pB = 0x7ffc6d94d50c
*/
```
