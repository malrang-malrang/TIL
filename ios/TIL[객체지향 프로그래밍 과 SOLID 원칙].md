# Today I Learned.

## 학습내용
# 객체지향 프로그래밍 과 SOLID 원칙

## 객체지향 프로그래밍
**객체란?**
- 인간이 분명하게 인지하고 구별할 수 있는 물리적인 또는 개념적인 경계를 지닌 어떤 것.
- 하나의 개별적인 실체로 식별 가능한 물리적인 또는 개념적인 사물은 어떤것이라도 객체가 될 수 있다.
- 상태(state), 행동(behavior), 식별자(identity)를 지닌 실체.

**객체지향 패러다임 이란?**
- 인간이 인지할 수 있는 다양한 객체들이 모여 현실 세계를 이루는 것처럼 소프트웨어의 세계 역시 인간이 인지할 수 있는 다양한 소프트웨어 객체 들이 모여 있다는 믿음에서 출발한다.
- 객체지향 패러다임의 목적은 현실세계를 기반으로 새로운 세계를 창조하는 것이다.

**객체지향 프로그래밍이란?**
- 프로그래밍에서 필요한 데이터를 추상화시켜 상태와 행위를 가진 객체를 만들고 그 객체들 간의 유기적인 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다.
- 객체지향의 본질은 **협력하는 객체들의 공동체 를 창조하는 것**이다

**객체지향을 이해하기 쉬운 이유?**
- 객체지향을 직관적이고 이해하기 쉬운 패러다임이라고 말하는이유는 객체지향이 세상을 자율적이고 독립적인 객체들로 분해할 수 있는 인간의 기본적인 인지 능력에 기반을 두고 있기 때문이다.

**객체지향 설계의 핵심**
- 협력을 구성하기 위해 적절한 객체를 찾고 적절한 책임을 할당하는 과정이다.
- 객체지향 패러다임 관점에서 핵심은 역할, 책임, 협력이다

## 객체 지향 프로그래밍에서 객체의 책임, 역할, 협력의 의미
### 1. 책임(responsibility)
- 객체가 협력에 참여하기 위해 수행하는 로직 또는 행동
- 객체의 책임은 무엇을 알고 있는가? / 무엇을 할 수 있는가 로 구분된다.

**아는것**
- 사적인 정보에 관해 아는것
- 관련된 객체에 관해 아는것
- 자신이 유도하거나 계산할 수 있는것에 대해 아는것

**하는것**
- 객체를 생성하거나 계산을 수행
- 다른 객체의 행동을 시작시키는 것
- 다른 객체의 활동을 제어하고 조절

### 2. 협력(collaboration)
- 객체들이 애플리케이션의 기능을 구현하기 위해 수행하는 상호작용

**객체간의 협력의 특징**
- 메세지 전송은 객체 사이의 협력을 위해 사용하는 유일한 커뮤니케이션 수단이다
- 외부의 객체는 오직 메세지만 전송할 수 있다
- 메세지를 수신한 객체는 어떻게 처리할지 스스로 결정한다
    - 객체는 자신의 일을 스스로 처리할 수 있는 자율적인 존재여야 한다.

### 3. 역할(role)
- 객체가 어떤 특정한 협력 안에서 수행하는 책임의 집합
- 역할을 통해 유연하고 재사용 가능한 협력을 얻을수 있다

예를 들자면 영화예메 시스템에서 할인 요금을 계산하는 객체가 2개 있다.
요녀석들을 모두 사용하기보다는 하나의 슬롯으로 구성하여 교대로 바꿔낄수있게 구성한다. 
여기서 슬롯이 역할을 의미하며 역할을 구현하는 일반적인 방법은 추상 클래스, 프로토콜 인터페이스를 사용한다.

## 객체지향 생활 체조 원칙
1. 한 메서드에 오직 한 단계의 들여쓰기만 합니다
2. else 표현을 사용하지 않습니다
3. 모든 원시 값과 문자열을 포장합니다
4. 한 줄에 점을 하나만 사용합니다
5. 이름을 줄여 쓰지 않습니다(축약 금지)
6. 모든 엔티티를 작게 유지합니다
7. 3개 이상의 스위프트 기본 데이터타입(Int, String, Double 등) 프로퍼티를 가진 타입을 구현하지 않습니다
8. 일급 콜렉션을 사용합니다(내부에 )
9. getter/setter를 구현하지 않습니다

## 객체지향 5원칙(SOLID)
- SRP (단일책임의 원칙: Single Responsibility Principle)
    - 작성된 클래스는 하나의 기능만 가지며 클래스가 제공하는 모든 서비스는 그 하나의 책임(변화의 축: axis of change)을 수행하는 데 집중되어 있어야 합니다
- OCP (개방폐쇄의 원칙: Open Close Principle)
    - 소프트웨어의 구성요소(컴포넌트, 클래스, 모듈, 함수)는 확장에는 열려있고, 변경에는 닫혀있어야 합니다.
- LSP (리스코브 치환의 원칙: The Liskov Substitution Principle)
    - 서브 타입은 언제나 기반 타입으로 교체할 수 있어야 한다. 즉, 서브 타입은 언제나 기반 타입과 호환될 수 있어야 합니다.
- ISP (인터페이스 분리의 원칙: Interface Segregation Principle)
    - 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야 합니다.
- DIP (의존성역전의 원칙: Dependency Inversion Principle)
    - 구조적 디자인에서 발생하던 하위 레벨 모듈의 변경이 상위 레벨 모듈의 변경을 요구하는 위계관계를 끊는 의미의 역전 원칙입니다.

### SOLID원칙의 장점
- 1. 재사용과 유지보수가 쉬운, 변경에 유연한 코드를 가지게 된다.
    - 이는 튼튼한 소프트웨어를 만들 수 있게 하며, 높은 확장성을 가지게 합니다.

- 2. 높은 응집력과 낮은 결합도(High Cohesion, Low coupling)

그리고 SOLID 원칙을 따르게 되면 아래 세 가지 문제를 해결할 수 있게 된다.
- 1. Fragility:
    - 작은 변화가 버그를 일으킬 수 있는데, 테스트가 용이하지 않아 미리 파악하기 어려운 것
- 2. Immobility: 
    - 재사용성의 저하. 불필요하게 묶인(coupled) 의존성 때문에 재사용성이 낮아진다. 
- 3. Ridgidity:
    - 여러 곳에 묶여 있어서 작은 변화에도 많은 곳에서 변화(노력)가 필요하다. 

## SOLID원칙의 예시
### 1. 단일 책임의 원칙 (SRP: Single Responsibility Principle)

- 하나의 객체는 하나의 책임을 가져야한다.
- 하나의 class가 여러 기능을 담당하면 안된다.

```swift
class ShoppingMall {
    
    func ShoppingMallSales() -> Int? {
        let sales = coffeeShop.coffeeShopSales() + clothingStore.clothingStoreSales() +
        restaurant.restaurantSales()
        
        return sales
    }
    
    func coffeeShopSales() -> Int {
        return 0
    }
    
    func clothingStoreSales() -> Int {
        return 0
    }
    
    func restaurantSales() -> Int {
        return 0
    }
}
```
위의 예시로 보게되면 ShppingMall 에는 여러가지 가게들이 입점 해있는 상태.

쇼핑몰은 입점해있는 모든 가게의 매출을 더해 집계할수 있는 기능을 가지고 있다.

지금은 예시로서 내부 구현을 하지 않은상태이지만 구현하게된다면 각각 해야하는 일들이 많아지게되고 ShoppingMall 의 Class 또한 덩치가 매우 커지게 된다.

이것은 SRP를 지키지 않는 코드로 하나의 클래스가 하나의 책임을 갖도록 하려면 기능을 분리해야한다.

```swift
class ShoppingMall {
    let coffeeShop = CoffeeShop()
    let clothingStore = ClothingStore()
    let restaurant = Restaurant()
    
    func ShoppingMallSales() -> Int? {
        let sales = coffeeShop.coffeeShopSales() + clothingStore.clothingStoreSales() +
        restaurant.restaurantSales()
        
        return sales
    }
}

class CoffeeShop {
    func coffeeShopSales() -> Int {
        return 0
    }
}

class ClothingStore {
    func clothingStoreSales() -> Int {
        return 0
    }
}

class Restaurant {
    func restaurantSales() -> Int {
        return 0
    }
}
```
요렇게 각각의 책임을 클래스에 넘겨주는 식으로 작성하면 하위 클래스들의 테스트도 용이해지게 된다!

### 2. 개방-폐쇄의 원칙 (OCP: Open-Closed Principle)
- 소프트웨어 엔티티(클래스, 모듈, 함수 등)들은 확장에는 열려있어야 하지만, 변경에는 닫혀있어야 한다.

 확장에 열려있다는 것은, 수고롭지 않게 클래스의 기능을 확장할 수 있다는 것이다. 
 
또한 변경에 닫혀있다는 것은, 기존에 구현되어 있는 것들을 바꾸지 않고 클래스를 확장할 수 있어야 한다는 것이다. 

이러한 특성들은 "추상화" 를 통해 이루어질 수 있다.(Swift 에서는 주로 Protocol 사용한다.)

```swift
class Fruits {
    let 딸기 = "딸기"
    let 바나나 = "바나나"
}

class FruitStore {
    var 과일들: [Fruits] = [Fruits(), Fruits()]
}
```
위의 코드를 보면 과일들 이라는 프로퍼티 내부에는 Fruit 타입만 저장될수있다.
지금은 과일의 종류가 딸기, 바나나 2개 밖에 없지만 과일의 종류를 더 추가하기 위해서는 Fruit 내부에 새로운 과일의 프로퍼티를 만들어 주어야한다.

예를 들자면 Fruits class 내부에 프로퍼티로 과일들을 추가 해주어야한다.
이렇게!
```swift
class Fruits {
    let 딸기 = "딸기"
    let 바나나 = "바나나"
    let 두리안 = "두리안"
}

class FruitStore {
    var 과일들: [Fruits] = [Fruits(), Fruits()]
}
```
이는 OCP 원칙에 위배되는 행위이다.
과일의 종류를 추가하기위해 기존에 구현되어있던 Fruits 타입을 수정해야 하기 때문이다.

이를 해결하고자 procotocol 을 활용해 해결할수 있다.

```swift
protocol Fruits {}

class 딸기: Fruits {
    let name = "딸기"
}

class 바나나: Fruits {
    let name = "바나나"
}

class 두리안: Fruits {
    let name = "두리안"
}

class FruitStore {
    var 과일들: [Fruits] = [딸기(), 바나나(), 두리안()]
}
```
Fruits class를 protocol로 변경해두었다.

이렇게 수정한다면 Fruits내부를 수정하지 않고 새로운 class를 만들어 Fruits protocol을 채택하기만 하면된다!

과일들 프로퍼티는 Fruits 프로토콜을 채택하는 녀석이라면 누구든 저장할수 있는 형태가 된다.

이렇게 수정하게 되면 기존에 작성된 코드를 수정하지 않고 과일의 종류를 추가할수 있게된다.

### 3. 리스코프 치환 원칙 (LSP: The Liskov Substitution Principle)
- 상위 타입(S)와 하위 타입(T) 사이에서, 프로그램의 속성(정확성, 수행하는 업무 등)의 변경 없이 상위 타입(S)의 객체를 하위 타입(T)로 교체(치환)할 수 있어야 한다는 원칙

상위 클래스의 자리에 하위 클래스를 치환해도 문제없이 수행되어야 한다는 의미이다.

이 원칙은 상속을 혼란스럽게 하지 않을 수 있도록 도와준다.

쉽게 얘기하면 부모 자식 관계에서 자식은 부모의 기능을 제한하면 안된다.

class 의 overriding 을 사용하게되면 리스코프 치환원칙을 위배할수있다.

리스코프의 치환 법칙하면 나오는 직사각형, 정사각형 문제를 가지고 한 번 살펴보자. 

정사각형은 직사각형 에 포함된다 할수있다.
직사각형과 정사각형을 구현할 때에 정사각형(square)이 직사각형(rectangle)을 상속 받았다고 해보자.

```swift
class Rectangle {
    var width = 10
    var height = 5
}

class Square: Rectangle {
    override var height: Int {
        didSet {
            height = width
        }
    }
}
```
직사각형의 경우 높이와 길이가 다를수 있지만 정사각형의 경우 길이와 높이가 같아야한다.

이를 해결하기 위해 override 하여 재정의 해주었다.
하지만 이렇게 재정의 하게되면 부모의 역활을 자식에서 대신하지못하는 상황이 발생된다. 
이는 LSP 원칙을 어기는 행위가 되어버린다. 

protocol 을 활용하여 해결할수 있다.

```swift
protocol Shape {
    var width: Int { get }
    var height: Int { get }
}

class Rectangle: Shape{
    var width = 10
    var height = 5
}

class Square: Shape {
    var width: Int = 5
    var height: Int = 5
}
```
위의 예시처럼 Rectangle(직사각형), Square(정사각형) 모두 Shape(모양) protocol 을 채택하게되면 프로토콜 내부의 프로퍼티 값(구현부)은 해당 프로토콜을 채택하는 하위 클래스에서 구현하도록 하면 LSP 의 원칙에 어긋나지 않는 프로그램을 설계할수 있게 된다.

이때 둘다 공통으로 가지고 있어야할 기능이 있다면 protocol 의 기본구현을 이용해 프로토콜을 채택한 하위 객체는 기본구현된 기능을 사용할수 있게된다.

### 4. 인터페이스 분리 원칙 (ISP: The Interface Segregation Principle)
- 클라이언트 객체는 사용하지 않는 인터페이스에 의존하면 안된다 
- 인터페이스를 작게 분리 유지해야 한다.

불필요한 인터페이스 요소들을 포함시키지 말라는 의미이다.

불필요한 요소들이 포함되면 복잡해지고 무거워진다.

이문제는 클래스나 프로토콜 모두에게 영향을 줄수있다.

**Protroco**
protocol의 경우를 보자!
```swift
protocol Gesture {
    func click()
    func doubleClick()
}
```
위와 같은 프로토콜을 만들었다 사용할때에는 위의 프로토콜을 채택하여 사용하면 click, doubleClick 기능을 구현하여 사용할수 있게된다.

하지만 click() 기능만 필요하다면 어떤가?
doubleClick()은 깡통으로 만들어둬야하나???

```swift
class Button: Gesture {
    func click() {
        print("클릭!")
    }
    
    func doubleClick() {}
}
```
이렇게 구현하는 것 자체가 ISP를 위반하는 무거운 인터페이스이다.
그렇기에 필요한 기능만 골라서 사용할 수 있도록 프로토콜을 나눠서 구현해주는 방식으로 구성해야한다. 

```swift
protocol click {
    func click()
}

protocol DoubleClick {
    func doubleClick()
}

class Button: click {
    func click() {
        print("클릭!")
    }
}
```
이렇게 나누어진 프로토콜중 필요한 프로토콜만 채택하여 원하는 기능만 구현하여 사용할수 있게된다.

**Class**
class의 경우를 보자!
```swift
class Malrang {
    let name = "malrnag"
    var age = "29"
    var hairStyle = "장발"
    
    func printProfile(profile: Malrang) {
    }
}
```
나의 정보를 포함하고 있는 Malrang 이라는 클래스가 있다.
printProfile 메서드는 나의 정보를 출력해주는 기능이다.
이때 사실 나이와 이름 은 알아야할 정보지만 헤어스타일은 굳이…출력하고 싶지않다.
그렇다면 매개변수로 Malrang 타입을 받아 모든 정보를 받아올 필요가 있을까?

이런 상황에서도 protocol 을 활용함으로써 printProfile 가 필요로 하는 정보만 전달해줄수 있도록 할수있다.

```swift
protocol Profile {
    var name: String { get }
    var age: Int { get  }
}

class Malrang: Profile {
    let name = "malrnag"
    var age = 29
    var hairStyle = "장발"
    
    func printProfile(profile: Profile) {
        print(profile)
    }
}
```
printProfile 의 매개변수에는 Profile 을 채택하는 것들로만 들어올수 있게 된다.

### 5. 의존관계 역전 원칙 (DIP: Dependency Inversion Principle)
-  상위 레벨 모듈은 하위 레벨에 의존하면 안된다. 두 개 모두 추상화된 인터페이스에 의존해야한다.
-  추상 개념은 세부 사항에 의존하면 안된다. 세부 사항이 추상 개념에 의존해야한다.

추상화는 위의 예시들에서 본 것 처럼 protocol 을 이용하는것 이것을 추상화에 의존했다 라고 할수 있겠다.

과일가게(FruitStore), 과일가게의 사장(JuiceMaker) 가 있다고 가정해보자.

```swift
class FruitStore {
    var fruitList = ["딸기", "바나나", "두리안"]
}

class JuiceMaker {
    let fruitStore = FruitStore()
    
    func printFruitList() {
        print(fruitStore.fruitList)
    }
}
```
위의 예시는 과일가게 사장이 과일가게의 인스턴스를 내부적으로 생성해 사용하게된다.

여기서 JuiceMaker 는 상위레벨, FruitStore는 하위레벨 인데 printFruitList 메서드를 사용하는 경우 JuiceMaker가 FruitStore를 의존하게 된다.

이러한 상활일때도 추상화 를 통해 상위레벨이 하위레벨을 의존하지 않게 바꿀수 있겠다.

```swift
protocol Fruits {
    var fruits: [String] { get }
    func printFruitList()
}

class FruitStore: Fruits {
    var fruits = ["딸기", "바나나", "두리안"]
    
    func printFruitList() {
        print(fruits)
    }
}

class JuiceMaker {
    let fruitList: Fruits
    
    init(fruitList: Fruits) {
        self.fruitList = fruitList
    }
    
    func getFruitList() {
        fruitList.fruitList()
    }
}

var 과일가게 = FruitStore()
var 과일가게사장 = JuiceMaker(fruitList: 과일가게)
과일가게사장.getFruitList()
```

위의 예시를 보게 되면 추상화(프로토콜)를 통해 서로가 프로토콜을 의존하도록 수정한예시 이다.

Fruits 라는 프로토콜을 정의하고 프로토콜 내부에 과일들이 저장된 fruits 프로퍼티와 과일들의 목록을 출력하는 기능을 fruits를 채택한 객체에서 구현하도록 하였다.

FruitStore 가 Fruits 프로토콜을 채택하고 필수 프로퍼티와 메서드를 구현해두었다.

JuiceMaker 는 FruitStore 를 소유하는 형태가 아닌 프로토콜을 채택하는 녀석을 저장할수있는 fruitList 라는 프로퍼티를 만들어 이를 활용하는 형태로 변경하였다.

fruitList는 Fruits를 채택하는 애들만 저장할수 있기 때문에 fruitList 는 Fruits 에 정의된 프로퍼티, 기능들을 사용할수 있게된다.

이렇게 fruitList가 FruitStore 를 의존하는 형태가 아닌 둘다 Fruits(프로토콜) 을 의존하는 형태로 수정함으로써 각각의 객체를 테스트할때도 수월해지며 fruitList 사용이 용이해진다.
