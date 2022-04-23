# Today I Learned.

## 학습내용

# Static Dispatch, Dynamic Dispatch

## Static Dispatch(컴파일 타임), Dynamic Dispatch(런 타임)
> 여기서 `Dispatch` 는 직역하면 보내다 라는 뜻인데, 
> 어떤 함수를 호출할것인지 결정 해주는것같다.
> 
> `swift` 에서는 `Static Dispatch`, `Dynamic Dispatch` 두가지 방식이 있다.

### Static Dispatch(컴파일 타임)
>요 방식은 컴파일 타임에 호출될 함수를 결정하고 런타임때에 그대로 실행시킨다.

### Dynamic Dispatch(런 타임)
>요 방식은 런타임에 호출될 함수를 결정한다.
>그렇기때문에 swift 에서 클래스마다 함수 주소값의 배열인 vTable(Virtual Dispatch Table) 을 유지하게된다.
>
>하위 클래스가 메서드를 호출할때 vtable 을 참조하여 실제 호출할 함수를 결정하며 이러한 과정이 런타임에 일어나기 때문에 성능 손해를 보게된다.

## class 에서의 vTable
>모든 클래스는 vTable 이란걸 가지고있다.
>
>클래스내부의 메서드들의 주소를 vTable 에 넣어 저장하고 있다.
>
>클래스의 상속을 이용해 상위클래스와 하위클래스로 나뉘어졌다면
하위클래스에서의 vTable 에는 상위클래스의 메서드의 주소값을 가지게된다.
>
>하위클래스에서 새로운 메서드를 정의했다면 하위 클래스의 vTable 에는 새로운 주소가 추가된다.
>
>하위클래스에서 상속받은 메서드를 오버라이딩 할 경우 하위클래스의 vTable 에 저장되어있단 상위 클래스의 메서드 주소가 오버라이딩된 메서드의 주소로 변경된다.
>
>그렇기때문에 어떤 메서드를 호출할것인지 런타임시점에 vTable 을 탐색해야한다. 
>런타임 과정에서 해당 클래스의 vTable 을 찾아 메모리 주소를 읽고 그 주소로 점프 해야하기 때문에 두개의 추가적인 명령이 필요해서 성능상 손해를 보는것이다.

## Reference Type 에서의 Dispatch
>`Reference Type` 은 참조타입 즉 `class` 는 상속의 기능이있다.
>
>그렇기 떄문에 하위클래스(서브클래스)에서 상위클래스의 함수를 호출할수 있기때문에 `Dynamic Dispatch` 를 사용한다.
>
**예시를 들어보자**
>```swift
>class Animal {
>    func howuling() {
>        print("우워어어")
>    }
>}
> 
>class Cow: Animal {
>}
>
>let malrang: Animal = Cow()
>malrang.howuling()
>```
>위의 예시 코드는 아무런 문제가 없다. 
>
>`Cow` 가 `Animal` 클래스를 상속받고 있지만 Overriding 하지 않았기 때문이다.
>
>`malrang`은 `Animal`타입이고 `Cow`인스턴스를 가리키고 있을때 `malrang`이 `Animal`의 `howuling()`메서드를 사용하면 무조건 `Animal` 타입의 `howuling()`이 호출될것이기 떄문이다.
>
>문제는 상속을 받은후 Overriding 했을때다.
>
>```swift
>class Animal {
>    func howuling() {
>        print("우워어어")
>    }
>}
> 
>class Cow: Animal {
>    override func howuling() {
>        print("음메~")
>    }
>}
>
>let malrang: Animal = Cow()
>malrang.howuling()
>```
>위의 예시 코드를 보면 `Cow`에서 `howuling()`을 `overriding`했기때문에 `malrang` 인스턴스는 `Cow` 의 `howuling()` 을 호출 해주어야 한다.
>
>예시 처럼 컴파일러는 클래스의 메서드가 하위클래스에서 `Overridng` 될 경우를 대비해 상위 클래스의 `howuling()`을 참조해야하는지 `Cow`의 `howuling()` 를 참조해야하는지 확인하는 작업을 **런타임**에 해주어야한다.
>
>따라서, `howuling()` 함수는 각 클래스마다 가지고있는 vTable 이란것 안에 함수의주소를 저장하고 실제 런타임 시점에 vTable 을 탐색하여 어떤 메서드가 호출되는지를 결정하게된다.
>
>처음 예시처럼 `Overriding` 되지 않을수도 있지만 `Overriding`될 가능성이 있기때문에 무조건 vTable 을 확인해서 참조한다고 할수있다.

### Reference Type 을 exension 한경우의 Dispatch
>`class` 를 `exension` 하여 메서드를 추가한 경우에는 
서브 클래스에서 오버라이딩이 불가능하다.
>
>`exension` 하여 메서드를 추가할때 @objc 키워드를 붙여주면 오버라이딩이 가능해진다!
>
>하지만 일반적으로는 `exension` 하여 메서드를 추가한경우 오버라이딩이 불가능하기 때문에 이와 같이 추가한 메서드는 `Static Dispatch` 로 동작하게된다.

## Value Type 에서의 Dispatch
>Value Type 에는 구조체, 열거형 이있으며 상속이 불가능 하기떄문에 오버라이딩될 가능성이없다. 
>
>그렇기 때문에 Static Dispatch 를 사용한다.
>```swift
>struct Animal {
>    func howuling() {
>        print("우워어어")
>    }
>}
>
>let malrang: Animal = Animal()
>malrang.howuling()
>```
>위의 예시처럼 `howuling()`은 항상 `Animal`라는 구조체의 `howuling()`이 호출될것이기 때문이다.
>
>런타임에 vTable을 탐색할 필요가 없음!

### Value Type 을 exension 한경우의 Dispatch
>상속의 가능성이 없기 때문에 `exension`(확장)을 해도 `Static Dispatch` 로 동작함!!

## Protocol에서의 Dispatch
>프로토콜은 기본적으로는 메서드의 선언부만 제공한다.
>실제 사용할때 프로토콜을 참조로만 사용할 경우 해당 인스턴스 타입에 맞는 메서드를 호출해야 해서 Dynamic Dispatch를 사용한다.
>
>```swift
>protocol Animal {
>    func howuling()
>}
>struct Cow: Animal {
>    func howuling() {
>        print("음메~")
>    }
>}
>struct Dog: Animal {
>    func howuling() {
>        print("멍멍")
>    }
>}
>let cow = Cow()
>cow.howuling()
>let dog = Dog()
>dog.howuling()
>```
>위의 예시처럼 프로토콜을 활용한경우 `cow`, `dog` 인스턴스모두 구조체 내부에있는 `howuling()`을 호출하기 때문에 `Static Dispatch` 로 작동되야 하는거 아니야?? 라고 생각할수있다.
>
>아래의 예시도 보자!
>```swift
>protocol Animal {
>    func howuling()
>}
>struct Cow: Animal {
>    func howuling() {
>        print("음메~")
>    }
>}
>struct Dog: Animal {
>    func howuling() {
>        print("멍멍")
>    }
>}
>let cow: Animal = Cow()
>cow.howuling()
>let dog: Animal = Dog()
>dog.howuling()
>```
>위의 예시처럼 `Protocol`을 타입으로 사용한경우 `class`의 예시처럼 `Animal` 타입 중에 어떤 타입의 의 `howuling()` 을 호출해주어야 할지 탐색해야하기 떄문에 `Dynamic Dispatch`를 사용한다.
>
>이때 프로토콜이 갖는 `vTable` 은 `Witness Table`라고 한다.

### Protocol 을 exension 한경우의 Dispatch
>프로토콜은 두가지 경우로 나눠 볼 수 있다.

**1. `Protocol`에 선언만 되어있는 메서드를`exension` 으로 `default`메서드를 구현한 경우**
>```swift
>protocol Animal {
>    func howuling()
>}
>extension Animal {
>    func howuling() {
>        print("음메")
>    }
>}
>struct Cow: Animal {
>}
>struct Dog: Animal {
>    func howuling() {
>        print("멍멍")
>    }
>}
>let cow: Animal = Cow()
>cow.howuling() // 음메
>let dog: Animal = Dog()
>dog.howuling() // 멍멍
>```
>위의 예시를 보면 `Cow`는 `howuling()`을 구현하지 않았기 때문에 `Animal`프로토콜이 `default` 로 구현해둔 `howuling()`을 호출하게된다.
>
>`dog`의 경우 프로토콜이 `extension`하여 `default`로 구현해둔 메서드가 있기는 하지만 직접 구현해둔 메서드가 우선순위가 더 높기 때문에 `dog`의 `howuling()`이 호출되었다.
>
>그렇기 때문에 어떤 `howuling()` 메서드를 호출해주어야 할지 단정지을수 없기 때문에 `Dynamic Dispatch` 로 동작하게된다.

**2. `Protocol`에 선언되어있지 않은 메서드를 `exension`으로 추가 구현한 경우**
>```swift
>protocol Animal {
>}
>extension Animal {
>    func howuling() {
>        print("음메")
>    }
>}
>struct Cow: Animal {
>}
>struct Dog: Animal {
>    func howuling() {
>        print("멍멍")
>    }
>}
>let cow: Animal = Cow()
>cow.howuling() // 음메
>let dog: Animal = Dog()
>dog.howuling() // 음메
>```
>위의 예시처럼 프로토콜에 선언되어있지 않은데 `extension` 으로 메서드를 추가 구현한 경우 `cow` 는당연히 구현해둔 메서드가 없으니 프로토콜의 `default` 메서드가 호출된다.
>
>하지만 `dog`의 경우는 자기 구조체 내부에 구현했음에도 불구하고 프로토콜의 `default` 메서드가 호출됐다;;
>
>`dog` 의 타입이 `Animal` 이라는 프로토콜의 타입이기 때문이다.
>프로토콜에 선언되지 않은 메서드를 `extension`하여 구현한 경우 해당 타입 내에서 똑같은 메서드를 구현하더라도 해당 프로토콜 타입에선 무조건 `extension` 으로 구현한 메서드만 실행되기 때문에 이때는 `Static Dispatch` 로 동작하게된다.

>```swift
>let dog: Dog = Dog()
>dog.howuling()
>```
>위의 예시처럼 타입을 변경해준다면 처음 예상했던 결과인 "멍멍" 이 출력되게 된다.
>
>위의 예시는 타입이 프로토콜 타입일때만 저렇게 작동한다! 명심하자!

## Dispatch 를 이해했으니 성능 최적화를 해보자!
**1. final**
>`final` 키워드를 사용해 `overriding` 의 가능성을 제거한다!
>
>`overriding` 의 가능성이 제거 되었으니 `Static Dispatch` 로 동작하겠지????

**2. private**
>private 키워드를 사용할경우 참조할수있는 곳이 현재파일에서만 가능하도록 제한된다.
>
>컴파일러는 `private` 키워드가 참조할수있는곳에서 오버라이딩이 되는지 안되는지를 탐색해보고 오버라이딩 되는곳이 없다면 `final` 이라고 추론해서 `Static Dispath` 로 동작하도록 함

**3. WMO(Whole Module Optimization)**
>Xcode 8 부터 자동으로 켜져있는 Xcode 설정중 하나다.
>
>swift 는 컴파일할때 모듈내의 파일을 하나씩 컴파일한다.
>
>그렇기때문에 A파일에있는 타입이 B파일에서 상속되는지 안되는지 컴파일 시점에 알수가 없다.
>
>다른 파일에서 상속 안받고 있다면 자동으로 final 로 추론해주면 좋을텐데 파일이 다르면 상속받는지 안받는지 알수가 없었음.
>
>이러한 문제를 개선하기위해 만들어진게 **WMO** 다.
>파일 하나씩을 컴파일 하는게 아니라 모듈 전체를 확인해서 컴파일한다.
>상속되고 있지 않으면 내부적으로 final 을 붙여 `Static Dispatch` 로 동작하게끔 바꿔준다.
>
>하지만 클래스의 접근제어자가 `internal` 일때만 가능하며 `open` 키워드라면 외부모듈에서도 접근할수 있기 때문에 WMO를 사용해도 `Dynamic Dispatch`로 동작한다

## 참고 자료
[개발자 소들이](https://babbab2.tistory.com/143?category=828998)
