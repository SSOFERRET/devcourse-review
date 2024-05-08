# :fire: 05/03 학습 내용

## :one: 클래스

- **사용자 정의 데이터 타입**
- 데이터와 메소드를 사용자인 내가 새로 정의한 데이터 타입이기 때문에 클래스를 추상적인 데이터 타입이라고 한다.
- 구조체와 비슷하다.
- 멤버 변수와 멤버 함수로 구성된다.
- 사물의 특성을 정리하여 필드와 메소드로 표현하는 과정이 추상화.
- 추상화된 결과를 클래스에 포함시키고, 스스로를 보호하는 것을 캡슐화.

- 클래스 선언 형식
  - 접근 지정자
    - public: 누구나 접근 가능
    - protected: 상속 관계에 있는 자식 클래스에서 접근 가능하다.
    - internal: 같은 어셈블리 내의 모든 클래스가 접근 가능하다.
    - protected internal: protected + internal
    - private: 내 클래스 내부에서만 접근 가능하고, 외부에서는 접근 불가하다.

```c#
using System;

class Dog{
    private int eyes, nose, mouse, ears;
    public void bark(){
        Console.WriteLine("멍멍");
    }
}

class MakeDog {
  static void Main() {
    Dog poppy = new Dog();
    poppy.bark();
  }
}

//멍멍
```

---

## :two: 생성자

- 생성자의 개념
  - 모든 변수는 선언이 되면 값을 초기화되어야한다. 객체 생성 시 초기화 전용 메소드를 제공하는데 이것이 바로 생성자, constructor이다.
  - 생성자는 객체 생성 시 자동으로 호출되는 메소드이다.

```c#
using System;

class Dog{
    private int eyes, nose, mouse, ears;
    public void bark(){
        Console.WriteLine("멍멍");
    }
    public Dog() { // constructor. 클래스명과 동일한 함수로 선언해주면 된다. 객체 생성 시 자동 호출되므로, 함수 호출 별개로 해줄 필요 없다.
        eyes = 0;
        nose = 0;
        mouse = 0;
        ears = 0;
    }
}

class MakeDog {
  static void Main() {
    Dog poppy = new Dog();
    poppy.bark();
  }
}
```

---

## :three: 상속성

- 상속의 체인에서 상위로 갈수록 추상적이고, 하위로 갈수록 구체적이다.
- 이미 완성된 클래스를 다른 클래스에 상속할 수 있다.
- 데이터 요소를 자식 클래스에게 상속하기 위해서는 접근 제한자를 private이 아닌 protected를 사용해야한다.

```
using System;

class Dog{
    protected int eyes, nose, mouse, ears;
    public void bark(){
        Console.WriteLine("멍멍");
    }
    public Dog() { // constructor. 클래스명과 동일한 함수로 선언해주면된다.
        eyes = 0;
        nose = 0;
        mouse = 0;
        ears = 0;
    }
}

class Poodle : Dog{
    public Poodle(){
        base.eyes = 2;
        Console.WriteLine("푸들 눈: {0}", eyes);
    }
}

class MakeDog {
  static void Main() {
    Dog poppy = new Dog();
    poppy.bark();
    Poodle pd = new Poodle();
  }
}

//멍멍
//푸들 눈: 2
```

---

## :four: 다형성

- 함수의 이름이 같더라도 전달인자의 타입이나 개수에 따라 구분된다.
- 오버로딩
  - 가적하다. 적재하다.
  - 이름이 같은 함수일지라도 전달인자 타입이나 개수가 다른 경우 다른 함수로 인식하여 동작한다.
  - 스타크래프트 저글링의 오버로드
```c#
using System;

public class Zerg{
    public void Overload(int zerggling){
        Console.WriteLine("저글링 {0} 마리", zerggling);
    }
    public void Overload(int zerggling, int hydra){
        Console.WriteLine("저글링 {0} 마리 + 히드라 {1}마리", zerggling, hydra);
    }
    public void Overload(int zerggling, int hydra, int lurker){
        Console.WriteLine("저글링 {0} 마리 + 히드라 {1}마리 + 럴커 {2}마리", zerggling, hydra, lurker);
    }
}

class HelloWorld {
  static void Main() {
    Zerg zerg = new Zerg();
    zerg.Overload(10);
    zerg.Overload(5, 5);
    zerg.Overload(5, 6, 7);
  }
}

/**
저글링 10 마리
저글링 5 마리 + 히드라 5마리
저글링 5 마리 + 히드라 6마리 + 럴커 7마리
*/
```
- 오버라이딩
