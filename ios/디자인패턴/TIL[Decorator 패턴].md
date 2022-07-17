# Today I Learned.

## 학습내용
# Decorator Pattern(데코레이터 패턴)

## Decorator Pattern이란?
데코레이터 패턴은 기존 객체가 가진 동작들을 포함하는 특수 Wrapper를 만들고 새로운 기능을 추가할 수 있는 디자인 패턴이다.

기존 객체를 감싸는 Wrapper를 만들고 해당 Wrapper객체에 추가 기능 들을 구현하는 패턴이기 때문에 Wrapper패턴 이라고도 불린다.

## Decorator Pattern은 언제 사용할까?
- 다른 객체들에 영향을 주지 않고 개별 객체에 기능들을 추가하고 싶을때 사용한다.
- 추가한 기능들은 언제든 없앨수 있다.
- 상속을 사용하여 기능을 확장하는 것이 힘들 경우 사용한다.

## Decorator Pattern 예시
![](https://i.imgur.com/mX4kegc.png)

어떠한 앱에서 중요 이벤트가 발생했을 때 사용자(User)에게 알림을 주는 객체가 있다고 가정해보자.

알림을 주는객체를 상속받는 Email 이라는 객체는 이메일을 통해 알림을 줄 수 있는 객체 였지만 시간이 지나 다양한 플램폼이 생겨나며 사용자들은 다양한 플랫폼에서 알림을 받고 싶어졌다.

SMS, 카카오톡 등의 메신저를 통해 알람을 받고 싶어진 사용자들을 위해 수정해야한다면 어떻게 해야할까?

가장 단순한 방법은 Notifier를 상속받는 서브 클래스들을 만들어 기능을 추가하는 방법이 있을수 있다. 다음과 같이 말이다.

![](https://i.imgur.com/PZ1EFBq.png)

하지만 또 시간이 지나 사용자 중 다양한 플랫폼에서 한번에 알람을 받기를 원하는 사용자가 생겼다. SMS, CacaoTalk 모두에서 말이다.

이를 위해 알림을 줄 플랫폼의 조합에 따라 상속(혹은 프로토콜을 채택)을 받아 처리하는것은 비효율적이다. 조합의 가짓수 만큼 객체를 생성해야하는 일이 발생되기 때문이다.
이러한 경우에 데코레이터 패턴을 사용할수 있다.

![](https://i.imgur.com/23ky9CO.png)

따라서 상속말고 새로운 객체를 만들고 기존 알림을 보내는 객체들을 참조만 해주면 기존 객체의 기능도 모두 할 수 있고 새로운 객체에 기능을 추가할 수도 있게된다.

위의 구조를 데코레이터 패턴으로 변경해 보자.
![](https://i.imgur.com/uHkIZ0I.png)

여기서 새로운 객체(Decoator)를 채택하는 객체를 Wrapper라고도 부르며 Wrappr도 기존 객체(Notifier)의 인터페이스(NotifierComponent)를 따르게 하면 데코레이터 패턴이 된다.

가장 하단에 존재하는 데코레이터가 실제로 작업하는 객체과 된다.
여러개의 플랫폼에 알림을 보내고 싶을때도 각각의 데코레이터 객체만 새로 만들고 처리하면 쉽게 사용할 수 있게 된다.

## 데코레이터 패턴을 구현해보자.
```swift
protocol NotifierComponent {
    func sendnNotify(message: String)
}

class Notifer: NotifierComponent {
    func sendnNotify(message: String) {
        print("\(message) 전송완료")
    }
}
```
요렇게 구현해서 App에서 알림을 줄수 있도록 사용해왔지만 추가적인 플랫폼에도 알람을 보내고 싶어졌다.

기존 NotifierComponent를 준수하는 Decorator 프로토콜을 만들어준다.

```swift
protocol Decorator: NotifierComponent {
    var wrapper: NotifierComponent { get set }
    init(notifier: NotifierComponent)
}
```
Decorator는 Wrapper의 역할을 한다고 했으니 객체를 하나 참조해줄 wrapper 가 있을 공간을 만들어 줘야한다.

그뒤 Decorator 프로토콜을 채택하는 Email, Sms, CacaoTalk을 만들어 준다.

```swift
class Email: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getEmailAdress()
        print("\(to)에게 Email로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    private func getEmailAdress() -> String {
        print("메세지를 보낼 Email 주소를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}
```

```swift
class Sms: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getPhoneNumber()
        print("\(to)에게 SMS로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    private func getPhoneNumber() -> String {
        print("메세지를 보낼 전화번호를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}
```

```swift
class CacaoTalk: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getId()
        print("\(to)에게 카카오톡으로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    func getId() -> String {
        print("메세지를 보낼 카카오톡 ID를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}
```

Email에는 Email주소를 입력받는 기능, Sms에는 전화번호를 입력받는 기능, CacaoTalk에는 카카오톡 아이디를 입력받는 기능이 추가되었다.

유저 에게 message를 전송해보자!

Notifier를 생성 한뒤 알림을 전송하는 메서드 를 호출하면 요렇게 메세지가 전송되는 모습을 볼수있다.
![](https://i.imgur.com/IJWWzjh.png)

SMS를 보내고 싶을때는 Sms를 생성 한뒤 메서드를 호출하면 요렇게 
![](https://i.imgur.com/JkJnp6Q.png)

전화 번호를 입력한뒤 메세지를 전송하는 로직이 실행된 것을 볼수 있다.

그럼 만약 Email, Sms, CacaoTalk 모두 보내고 싶다면 어떻게 할까?

![](https://i.imgur.com/ALRzLtI.png)

위와 같이 다중 래핑하여 한번에 여러 개의 플랫폼에 한번에 보낼수 있게된다.

CacaoTalk의 send()메서드 부터 차례 대로 호출되는 모습을 볼수 있다.

## 데코레이터 패턴의 장단점
### 장점
1. 상속을 통한 하위 클래스를 만들지 않고 객체의 기능을 확장할수 있다.
2. 객체를 여러 데코레이터로 래핑하여 여러 동작을 합칠 수도 있다.
3. 객체 지향 프로그래밍에서 중요한 단일 책임 원칙을 지킬 수 있다.

### 단점
1. Wrapper Stack 에서 특정 Wrapper를 제거하는것이 어렵다.
2. 데코레이터 기능이 데코레이터 Stack 순서에 의존해야한다.
3. 코드가 복잡해질 수 있다.

## 전체코드
```swift
protocol NotifierComponent {
    func send(message: String)
}

class Notifer: NotifierComponent {
    func send(message: String) {
        print("\(message) 전송 완료")
    }
}

protocol Decorator: NotifierComponent {
    var wrapper: NotifierComponent { get set }
    init(notifier: NotifierComponent)
}

class Email: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getEmailAdress()
        print("\(to)에게 Email로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    private func getEmailAdress() -> String {
        print("메세지를 Email주소를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}

class Sms: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getPhoneNumber()
        print("\(to)에게 SMS로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    private func getPhoneNumber() -> String {
        print("메세지를 보낼 전화번호를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}

class CacaoTalk: Decorator {
    var wrapper: NotifierComponent
    
    required init(notifier: NotifierComponent) {
        self.wrapper = notifier
    }
    
    func send(message: String) {
        let to = getId()
        print("\(to)에게 카카오톡으로 \(message)를 전송합니다.")
        self.wrapper.send(message: message)
    }
    
    func getId() -> String {
        print("메세지를 보낼 카카오톡 ID를 입력하세요 : ",separator: "", terminator: "")
        return readLine()!
    }
}
```

## 참고한 문서및 자료
https://refactoring.guru/design-patterns/decorator
https://icksw.tistory.com/244
