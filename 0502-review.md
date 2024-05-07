# :fire: 05/02 학습 내용

## :one: 함수포인터

- 함수명 앞에 *를 붙여주면 함수 포인터가 선언된다.
- 함수 포인터도 포인터이므로 주소값을 저장한다.

```
//자료형(*함수 표인터 이름)(인자 목록)

int (*func)(int a);
```

![1](https://github.com/SSOFERRET/devcourse-review/assets/148465774/5e59fb3e-91ef-46c6-affa-5ddae90a4fef)

- 함수 포인터의 사용 이유
  - 메모리 크기 및 위치가 결정되는 시점은 컴파일 타임 또는 런타임시점이다.
    - 컴파일 타임 시점: 정적 바인딩
    - 런타임 시점: 동적 바인딩
  - 함수 포인터의 사용은 프로그램의 유연한 확장성을 제공한다.
  - 예) VS code extension는 플러그인 방식으로 동작하는데, 이는 변경 사항 생길 때마다 매번 컴파일해야할 필요가 없다.
---

## :two: 구조체

- 하나 이상의 서로 다른 종류의 변수를 묶어서 새로운 데이터 타입을 정의하는 것.
- 추상적이다.

- 사용 이유: 연관된 변수를 하나로 묶어서 관리함으로써 데이터 관리에 유용하다.
- 기본 형태
```c
struct student {
  char name[10];
  int age;
  int height;
};
```

- 접근하려면 직접 접근자(.)를 통하면 된다.
```
#include <stdio.h>
#include <string.h>

struct student{
    char name[10];
    int age;
    int height;
}st1;

int main()
{
    strcpy(st1.name, "이창현");
    st1.age = 20;
    st1.height = 178;
    
    printf("이름: %s, 나이: %d, 키: %d\n", st1.name, st1.age, st1.height);
}
```

❔ **평가 문제 >>** 우체국 택배 물건의 종류, 무게, 높이 정보를 가지고 있는 구조체를 선언한 후, 구조체 멤버 값을 사용자로부터 입력 받은 후 출력하는 프로그램 작성하라.
