# Today I Learned

# 학습 내용
## 클래스 상속과 초기화 (Class Inheritance and Initialization)

모든 클래스는 적어도 하나의 지정된 초기화 구문을 가지고 있어야 한다.
상위 클래스와 상속받은 하위 클래스 모두 모든 프로퍼티에 초기값을 넣어주거나 초기화 과정중 초기값이 할당되어야 한다. 

Swift는 모든 저장된 프로퍼티가 초기값을 받을 수 있도록 클래스 타입에 2가지의 초기화 구문을 제공합니다.

### 지정 초기화
지정된 Initializer 구문 (Designated initializers) 은 클래스에 주 초기화 구문입니다.
지정된 Initializer 구문은 해당 클래스의 모든 프로퍼티(상속받은 프로퍼티 포함)에 초기값을 할당해줄수 있다.
지정 Initializer 구문 내부에 상위 클래스의 초기화 구문을 호출하여 하위 클래스의 인스턴스 생성시 상위 클래스의 프로퍼티에도 초기값을 할당하여야한다.(super.inint())
![](https://i.imgur.com/qFj2Jkf.png)


### 편의 초기화
편의 Initializer 구문 (Convenience initializers) 은 생략이 가능하며 지정 초기화 와 같이 사용할수도 있다.
클래스 내부에 Initializer, 편의 Initializer 중복으로 있을수 있다.
편의 Initializer 내부에 지정 Initializer 를 호출한후
지정초기화 에 들어갈 파라미터 를 연산된 값을 넣어주거나 할때 사용하면될듯 하다.
![](https://i.imgur.com/Ko4y78I.png)


## observe()메서드 사용 방법
- 사용하려면 NSObject 를 상속 받아야함.
- 그런고로 class 에서만 사용가능!

1. observe()메서드 사용
2. (keyPath:: 어떤 프로퍼티를 감시할거냐?) \ 이후 관찰할 프로퍼티 작성(예시에는 name과 age가 있을수 있다.)
3. (options:)사용할 옵션을 정할수있음
- .new: 새로운값(변경후 값)
- .old: 예전값(변경전 값)
- .inital: 초기화 과정도 변경 된 것으로 간주함
- .prior: 이전 상태와 지금 상태를 Bool 값으로 나타내준다.
4. object: 현재 malrang 의 값들이 들어가 있으며 change 키워드를 작성해 option을 사용할수 있게 해준다.

```swift
import Foundation
class Person: NSObject {
    @objc dynamic var name: String
    @objc dynamic var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

let malrang = Person(name: "malrang", age: 29)

malrang.observe(\.name, options: [.new, .old]) { (object, change) in
    print(object.name)
    print(change.oldValue)
    print(change.newValue)
}

malrang.name = "말랑"
```
![](https://i.imgur.com/yiWrDYW.png)

위의 예시는 옵션에 `new`,`old`를 사용한 모습이다.
콘솔창을 보게되면 현재 malrang 인스턴스의 name 이 출력되고
변경전 name 과 변경후 name 이 출력되는것을 확인 할수 있다.

```swift
malrang.observe(\.name, options: [.new, .old, .initial]) { (object, change) in
    print(object.name)
    print(change.oldValue)
    print(change.newValue)
}

malrang.name = "말랑"
```
![](https://i.imgur.com/Pls5IB2.png)
위의 예시는 .initial 옵션을 사용한 경우

인스턴스를 초기화 한것도 변경한것으로 적용하여
malrang의 초기네임인 malrang 이 출력되고
변경전 값은 없으니 nil 로 출력되며 
변경된 값은 malrang 을호 출력된 모습이다.

그후 malrang의 name을 말랑으로 변경하게되어 
말랑이 출력되고 변경되기전 과 변경된후의 name 이 출력된것을 확인할수있다.

```swift
malrang.observe(\.name, options: .new) { (object, change) in
    print(object.name)
    print(change.oldValue)
    print(change.newValue)
}

malrang.name = "말랑"

```
옵션을 new 만 사용한경우
![](https://i.imgur.com/DXdLMjp.png)
oldValue 를 사용해도 old 옵션을 사용하지 않았기 때문에
nil 로 출력된다.

반대의 경우도 마찬가지다.

```swift
malrang.observe(\.name, options: .old) { (object, change) in
    print(object.name)
    print(change.oldValue)
    print(change.newValue)
}

malrang.name = "말랑"
```
![](https://i.imgur.com/9l3c4A7.png)
newValue 를 사용해도 new 옵션을 사용하지 않았기 때문에
nil로 출력된다.

```swift
malrang.observe(\.name, options: [.new, .old, .prior]) { (object, change) in
    print(object.name, change.isPrior)
}

malrang.name = "말랑"
```
![](https://i.imgur.com/6QdumuG.png)
prior 옵션을 사용할경우 변경전 값과 변경된 값을 Bool 값으로 반환해준다.
변경전 값은 true 이며 
변경후 값은 false 다.
