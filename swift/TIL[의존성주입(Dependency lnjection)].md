# Today I Learned.

## 학습내용
## 의존성주입(Dependency lnjection)
> 의존성 주입은 하나의 객체가 다른 객체의 의존성을 제공하는 기술이다.
>
>### 의존성이란?
>어떤 객체가 내부에서 생성 하여 가지고있는 객체를 의존성(Dependency)라고 한다.
>
>**예시코드**
>```swift
>class Car {
>    var wheel: Wheel = Wheel()
>}
>
>class Wheel {
>    var weight = 10
>}
>```
>위의 예시코드를 보면 Car 클래스 내부에서 사용된 Wheel 이 의존성이다.
>Car 클래스는 Wheel 이라는 클래스에 의존하고 있는것이다.
>
>### 의존성 주입이란?
>말그대로 의존성을 주입 시킨다는뜻이다.
>내부에서 초기화가 이루어지는것이 아니라 외부에서 객체를 생성하여 내부에 주입해주는것.
>
>**예시코드**
>```swift
>class Car {
>    var wheel: Wheel
>
>    init(wheel: Wheel) {
>        self.wheel = wheel
>    }
>}
>
>class Wheel {
>    var weight = 10
>}
>
>let myWheel = Wheel()
>let myCar = Car(wheel: myWheel)
>```
>위의 예시코드에서 init() 을 활용해 의존성 주입을 해줄수있다.
>Car 클래스를 초기화 할때 myWheel 객체를 Car 클래스의 wheel 프로퍼티에 주입해준것이다.
>
>### 의존성주입 왜씀??
>객체간의 결합도를 낮추기 위함이다.
>객체간의 결합도가 낮으면 리팩토링이 쉽고 테스트 코드 작성이 쉬워진다.
>그리고 상태값도 다른 객체가 알지 못하도록 접근제어를 걸어줄수도 있게된다!
>
