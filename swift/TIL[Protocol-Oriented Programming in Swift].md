# Today I Learned.

## 학습내용
# Protocol-Oriented Programming in Swift 
>애플이 2015년 WWDC 에서 스위프트를 발표하면서 스위프트는 프로토콜 지향언어(Protocol Oriented Programming)라고 발표했다.
>
>프로토콜 지향 프로그래밍 POP 라는 말을 게속 들어왔지만 구체적으로 이게 무엇인지 모른다..! 알아보자..!

### POP(Protocol Oriented Programming) 가 생겨난 계기
>객체 지향 프로그래밍 패러다임 은 사물을 객체로 만들때 속성값을 가지게 된다.
>
>이후 클래스가 상속이 거듭될수록 하위 클래스는 필요하지도 않은 부모클래스, 조상클래스들의 속성이나 행위를 가지게된다.
>
>또 가장 큰 단점은 다중상속이 불가하고 단일상속만 가능하다는점이다.
>
>이러한 단점들을 보완하기위해 프로토콜 지향 프로그래밍이 등장하게 된것같다.
>
>POP 는 필요한 부분만 프로토콜로 분리해서 만들수 있고 다중 프로토콜을 구현해 다중채택이 가능하다.
>
>프로토콜은 class, struct, enum 에서 채택하여 적용할수 있기 때문에 확장 부분에서 OOP보다 유연하다고 할수있다.
>
>스위프트는 표준 라이브러리 내부를 보면 class 로 구현된 타입보다 struct 로 구현되어있는 타입이 많은데, 상속이 불가능한 struct 들이 공통 기능을 가질수 있게하는 방법에는 프로토콜의 익스텐션 으로 구현할수있다.

### Protocol 과 extension 의 간단한 설명
**Protocol**
>특정 역할을 수행하기 위한 메서드, 프로퍼티 등 사항들을 한곳에 모아 놓은 청사진.
>
>프로토콜에는 채택시 반드시 구현해야하는 내용들이 정의되어있다.

**extension**
>기존 타입의 기능을 확장하도록 도와주는 기능.

## 프로토콜의 초기구현
>특정 프로토콜을 정의하고 여러 타입에서 해당 프로토콜을 채택한후 각 타입마다 똑같은 내용들을 구현해야 한다면 엄청많은 중복 코드들이 생겨날것이다.
>
>추후 유지보수를 하게된다면 프로토콜을 채택한 모든 데이터 타입의 코드를 일일이 변경시켜줘야할것이다.
>
>이러한 단점을 보완하고자 애플에서 제공하는 기능이 프로토콜에 익스텐션을 적용한 프로토콜 초기구현 이다.
>
>**1. 타입과 타입이 채택할 프로토콜을 구현해보자!**
>```swift
>protocol Infomation {
>    var name: String { get }
>    var age: Int { get }
>    
>    func getName() -> String
>    func getAge() -> Int
>}
>
>struct Person: Infomation {
>    let name: String
>    
>    var age: Int
>    
>    func getName() -> String {
>        return name
>    }
>    
>    func getAge() -> Int {
>        return age
>    }
>}
>```
>`Infomation`프로토콜에는 채택할시 필수로 구현해야할 속성과 행위가 정의되어있다.
>`Person` 구조체는 `Infomation`프로토콜을 채택했기 때문에 내부에 속성값인 `name`, `age`, 그리고 메서드 `getName()`, `getAge()` 를 정의해주었다.
>
>하지만 `Infomation` 을 채택하는 모든 타입들 내부에 똑같은 속성값, 행위들을 구현하게되면 중복코드가 어마어마할거다..
>
>여기서 우리는 위에 설명한데로 `extension` 을 활용해 프로토콜 초기구현을 하여 `Person`타입의 행위들을 구현하지 않아도 되도록 수정해보자!
>
>**2. 프로토콜을 extension 하자!**
>```swift
>protocol Infomation {
>    var name: String { get }
>    var age: Int { get }
>    
>    func getName() -> String
>    func getAge() -> Int
>}
>
>extension Infomation {
>    func getName() -> String {
>        return name
>    }
>    
>    func getAge() -> Int {
>        return age
>    }
>}
>
>struct Person: Infomation {
>    let name: String
>    
>    var age: Int
>}
>
>var malrang = Person(name: "malrang", age: 29)
>malrang.getAge()  //29
>malrang.getName() //malrang
>```
>`Infomation` 프로토콜을 확장하여 프로토콜을 채택하는 타입이 필수로 구현해야하는 메서드 `getName()`, `getAge()` 의 내부로직을 정의해 주었다.
>
>그후 `Person` 구조체 내부에서 `getName()`, `getAge()`메서드를 구현해주지 않았다.
>
>하지만 예시코드 맨아래의 코드와 실행결과를 보면 `Infomation` 프로토콜의 `extension` 에 정의된 메서드들을 `Person` 타입의 인스턴스가 호출해주는 모습을 볼수있다.
>
>이렇게 `Infomation` 프로토콜을 준수하는 구조체에서 해당 메서드를 따로 구현할필요 없이 사용할수 있게된다.
>
>마치 class 의 상속과 비슷한 모습? 이다.(실제로는 조금 다르지만)
>
>이렇게 하나의 프로토콜을 정의하고 초기 구현을 해둔다면 여러 타입에서 해당 기능을 사용하고 싶을때 해당 프로토콜을 채택하기만 하면 된다!

## protocol 의 generic
>프로토콜을 채택하는 타입이 제네릭을 사용할경우!
>프로토콜을 좀더 유연하게 사용할수있도록 정의해줄수있다.
>키워드는 `associatedtype` 이다!
>```swift
>protocol Listable {
>    associatedtype Element
>    var items: [Element] { get set }
>    
>    mutating func append(with item: Element)
>    
>    mutating func removeLast() -> Element
>}
>
>extension Listable {
>    mutating func append(with item: Element) {
>        items.append(item)
>    }
    
    mutating func removeLast() -> Element {
        return items.removeLast()
    }
}

struct List<Element>: Listable {
    var items: [Element]
}

var intList = List<Int>(items: [1, 2, 3])
var stringList = List<String>(items: ["1", "2", "3"])

intList.append(with: 5)
```
> 코드에대한 설명을 해보자면 프로토콜 내부에 작성된 키워드 `associatedtype`는 채택한 타입에서 사용되는 제네릭 타입이다!
> 
>무슨 타입이 될진 아직 알수 없지만 그 제네릭 값을 가진 프로퍼티, 메서드를 구현해야한다고 정의한것이다!!
>
>그후 `List` 구조체를 보면 제네릭을 사용하는 구조체이며 `Listable`프로토콜을 채택했다. 
>
>그럼 이전 예시 코드에서 처럼 프로토콜에 정의된 것들을 구현해주어야한다! 하지만 구현한 프로퍼티의 값은 제네릭 타입이다! 뭐가들어올지 모른다!!
>
>예시코드의 맨아래쪽을 보면 사용되는 모습을 볼수있다!
>
>`List` 구조체는 제네릭을 사용했기 때문에 초기화 할때 어떤 타입의 값을 사용할건지 정해줄수있다!
>제네릭에 Int 타입을 넣을경우 Int 타입으로 사용되고, String 타입을 넣게 될경우 String 으로 사용된다.
>
>굉장히 유연한 구조체가 되었다!

## POP 의 장점
1. 객체간의 결합도를 낮출수있다.
- 위의 예시들 말고도 객체간의 결합도를 낮추어 객체의 재사용성을 높일수있다. 
- A객체와 B객체가 있을때 객체들이 서로를 바라보는형태가 아닌 A객체, B객체 사이에 프로토콜을 구현해 프로토콜을 통해 객체끼리 소통하게 할수있다!

2. 값 타입의 상속 효과
- 값 타입임에도 불구하고 클래스처럼 공통된 기능들을 쉽게 구현할수 있다.

3. 수직구조 확장이 아닌 수평 구조의 확장
- class 의 경우 자식 클래스는 하나의 부모클래스만 상속할수있기 때문에 수직적인 구조를 갖게된다.
- 하지만 Protocol 을 활용하면 수직구조가 아닌 수평 구조로 기능을 확장할수 있다.

4. 프로토콜 의 제네릭
- 좀더 유연한 타입과 유연한타입의 확장을 위해 프로토콜에서 제네릭 기능을 제공하여 POP 의 장점이 늘어났다.
