# Today I Learned.

## 학습내용

# MetaType, self 와 Self

## MetaType 
> 공식문에서는 `Metatype` 은 타입의 타입이라고 정의 되어있다.
> 이게 대체 무슨소리일까..
>
>아래 예시를 보자
>```swift
>struct Person {
>    static let name = "malrang"
>    var age = 29
>}
>
>let malrang = Person()
>```
>지금껏 나는 `Person` 이라는 타입을 정의한후 
>`Person` 타입의 `malrang` 이라는 상수를 정의해주었다.
>`malrang` 은 `Person` 타입의 인스턴스다.
>하지만 여기서 `Person` 은 타입의 타입인 메타타입이라 한다.
>
>`malrang` 인스턴스의 타입의 타입
>그러니까 즉 타입 자체?본체?를 메타타입이라고한다.

### type(of:)
>요 메서드는 파라미터에 인스턴스를 넣게되면 인스턴스의 메타타입을 반환한다.
>아래 예시를보자
>```swift
>let malrang = Person()
>let malrangType = type(of: malrang) // Person.Type 
>malrangType.name                    // malrang
>```
>`type(of:)` 를 사용해서 `malrangType` 이라는 애한테 `Person` 타입을 할당해줬다. 인스턴스를 넘겨준게 아니라 타입 자체를, 메타타입을 넘겨준거다.
>그렇다면 `malrangType` 은 `Person` 타입의 타입 프로퍼티인 `name` 에 접근이 가능해진다! 
>그리고 `malrangType` 은 인스턴스가 아니기때문에 `age` 에는 접근이 불가능하다!!

### 정리해보자
>`Person` 타입은 `Person` 타입으로 생성한 인스턴스를 말하며 인스턴스 멤버를 사용가능하다!
>`Person` 타입의 타입(메타타입)은 `Person` 타입 그자체를 가르키는 타입으로 타입 멤버 사용이 가능하다!

### MetaType 을 나타내는 키워드 .Type
>위의 예시에서 메타타입을 설명했다.
>메타타입인지 아닌지 나타낼수있는 키워드 또한 존재하는데 키워드는 `.Type `이다.
>위의 예시코드에서 타입어노테이션을 작성한다면 아래와 같은 모습이된다.
>```swift
>struct Person {
>    static let name = "malrang"
>    var age = 29
>}
>
>let malrang = Person()
>let malrangType: Person.Type = type(of: malrang)
>malrangType.name
>```
>`malrangType` 의 타입은 `Person.Type` 로 명시해주었다.

### Protocol 의 MetaType 을 나타내는 키워드 .Protocol
>위의 예시에서 `struct`, `class` 같은 경우 메타타입을 나타내는 키워드는 `.Type` 이었지만 프로토콜에서의 메타타입을 나타내는 키워드는 `.Protocol` 이다.
>아래예시 처럼 사용할수 있겠다.
>```swift
>protocol Person { }
>
>let malrangType: Person.Protocol = Person.self
>```

## self 와 Self
>소문자 `self` 랑 대문자 `Self` 두개 다른거다 같은거아니다!!

### self(소문자)
>`self` 는 인스턴스에서 생성되며 해당 인스턴스를 나타낸다.
>```swift
>struct Person {
>    var age = 29
>    func printAge() {
>        print(self.age)
>    }
>}
>
>let malrang = Person()
>malrang.printAge()
>```
> 위의 예시에서 볼수있듯이 `printAge()` 메서드는 인스턴스 자기자신의 나이를 출력하는 메서드다.
> `printAge()` 내부의 `self` 는 자기자신을 가르키게 되는것이다.
> `age` 의 값이 29 가 아니고 다른값으로 변하게 되더라도 변한 값을 출력하게 될것이다.
> 사실 위의 예시 코드에서 `self` 가 없어도 되지만 명확성을 부여하기 위해 사용된다

#### 해당 타입의 메타타입 값을 얻기위해 사용된다.
```swift
struct Person {
    static let name = "malrang"
    var age = 29
    func printAge() {
        print(self.age)
    }
}
let malrang = Person.self
```
>위에서 사용된 `type(of:)` 메서드 처럼 상수에 타입이름과 `self` 키워드를 사용해서 메타타입 을 할당해줄수있다.
>
>하지만 `type(of:)` 를 이용한 방법과 `self` 를 이용한 방법에는 차이점이 있다.

#### Static Metatype, Dynamic Metatype
>`self` 를 이용해 만든 메타타입은 `Static Metatype` 이고
>`type(of:)` 를 이용해 만든 메타타입은 `Dynamic Metatype` 이다.
>
>둘의 차이점은 타입이 정해지는 시점이 다르다.
>`Static Metatype` 은 컴파일 시점에 타입이 정해진다.
>`Dynamic Metatype` 은 런타임 시점에 타입이 정해진다.

### Self(대문자)
>`Self` 는 상황에따라 의미가 다르다.
>구조체와 프로토콜을 예시로 들어보겠다.
>
>**구조체의 경우**
>현재 속한 코드의 `Type` 이 `Self` 의 `Type` 이 된다.
>```swift
>extension String {
>    func makeEmpty() -> String {
>        return String()
>    }
>}
>```
>`String` 을 확장해서 `makeEmpty()` 메서드를 정의해주었다.
>반환값은 `String` 이다.
>여기서 `Self` 를 사용할수도있다. 위의 설명에서 현재 속한 코드의 `Type` 이 `Self` 의 `Type` 이된다고했다.
>```swift
>extension String {
>    func makeEmpty() -> Self {
>        return Self()
>    }
>}
>``` 
>`makeEmpty()` 메서드는 현재 `String` 타입 내부에 있다.
>그러니까 현재 속한 코드는 `String` 내부에 속해 있기 때문에 `String` 대신 `Self` 를 사용할수있다.
>
>**프로토콜의 경우**
>프로토콜 내부의 `Self` 는 자기자신(프로토콜)이 아닌 자신을 채택한 타입을 의미하게된다.
>아래의 예시는 `Decodable` 이라는 프로토콜을 확장해서 메서드를 정의해주었다.
>```swift
>extension Decodable {
>    static func parse(_ name: String) -> Self? {
>        guard let asset = NSDataAsset(name: name) else >{
>            return nil
>        }
>        let jsonData = try? >JSONDecoder().decode(Self.self, from: asset.data)
>
>        return jsonData
>    }
>}
>```
>예시 코드를 보면 반환값이 `Self` 인걸 알수있다.
>위의 설명만 보면 이해하기 어려울것 요 메서드 가 사용되는 것을 보면 이해가 쉽다.
>```swift
>let expositionItems =[ExpositionItems].parse(JsonFile.items)
>```
>위의 예시 코드에서 `ExpositionItems` 는 `Decodable` 을 채택하는 모델이다. `ExpositionItems` 요녀석이 `Decodable` 의 `parse()` 메서드를 사용하게될경우 반환 타입은 무엇일까?
>
>조금전 설명에서 프로토콜에서의 `Self` 는 자신을 채택하는 타입의 타입이 된다고 했다.
>그러면 예상할수 있듯이 `expositionItems` 타입은 `[ExpositionItems]?` 가 된다.
