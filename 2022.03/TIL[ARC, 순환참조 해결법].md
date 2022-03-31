# Today I Learned

## 학습내용
참고한 문서
[ARC공식문서](https://bbiguduk.gitbook.io/swift/language-guide-1/automatic-reference-counting)
[제드블로그](https://zeddios.tistory.com/1213)

## ARC 자동 참조 카운팅 (Automatic Reference Counting)
swift는 앱의 메모리 사용량을 관리하기 위해 ARC 를 사용한다.
ARC 는 클래스 인스턴스가 더이상 사용되지 않을때 자동으로 메모리에서 해제 시켜준다.

ARC 는 해당 클래스의 RC(Reference Counting) 를 통해 사용하고있는지 사용되지 않는지 알수있고 이를 통해 자동으로 메모리에서 해제가 가능하게 된다.

### 클래스의 저장 방식
클래스를 인스턴스화 하게되면 stack 에는 heap 을 가르키는 포인터가 저장된다.

이때 heap 영역을 참조하게 되므로 RC 는 1 이된다.
RC 는 heap 영역에 같이 저장된다.
RC 를 측정하기 위해 메모리를 조금더 사용한다고 할수 있겠다.

![](https://i.imgur.com/Q8hPSjR.png)

새로운 상수 point2 를 만들어 point1 을 할당시 아래왜 같은 상황이 발생한다.

![](https://i.imgur.com/WhMsXGC.png)

새로운 상수를 만들었기 때문에 stack 에 메모리를 하나더 사용하게 되고 
이미 생성되어있던 heap 영역의 메모리를 참조하기 때문에 heap 영역에 추가되는 것은 없으나 이때 heap 의 RC 는 1 증가 하게 된다.

이때 RC는 2 이다.

> 상수 또는 변수에 클래스 인스턴스를 할당할 때 해당 인스턴스에 대한 강한 참조가 생긴다.

이때 강한 참조(Strong) 는 참조가 유지되는 동안 메모리 할당 해제를 허용하지 않는것을 뜻한다.

### 클래스 인스턴스 사이의 강한 참조 사이클 (Strong Reference Cycles Between Class Instances)

순환 참조 이녀석...

예시를 보자!

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

이렇게 두개의 클래스 가 있다.

```swift
var john: Person?
var unit4A: Apartment?
```

얘내를 인스턴스화 하게되면 위의 예시처럼 각각 힙영역에 한놈 스택에는 힙의 주소를 저장할 한놈 씩 저장되게 된다.

![](https://i.imgur.com/KMR1Iyy.png)

그런데 여기서 john, unit4A 의 프로퍼티인 apartment, tenant 에 서로를 연결시켜주면 ... 골때리는 상황이 생겨난다.

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```
그림으로 보면 이해가 쉽다!

![](https://i.imgur.com/OO9kNLY.png)

```swift
john = nil
unit4A = nil
```

이후 john, unit4A 의 값을 nil 을 넣어주면...
deinit 이 호출 되지 않는다..

![](https://i.imgur.com/F09NraJ.png)


위와 같은 골때리는 상황이 생긴다.
서로 heap 영역에서 서로를 참조하는 순환 참조 상태이기때문에 
RC가 0이 되지 않아 메모리에서 해제 되지 않는다.

이런 순환 참조를 해결하는 방법이 weak 와 unowned 를 사용하는 것 이다.

### 클래스 인스턴스 간의 강한 참조 사이클 해결 (Resolving Strong Reference Cycles Between Class Instances)

위에서 설명한 대로 해결 방법은 2가지 방법이 있다.

1. Weak References(약한참조)
2. unowned references(미소유 참조)

### 약한 참조 (Weak References)
약한참조 weak 은 말그대로 약한 참조이며 RC 를 증가시키지 않는 방법이다.

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```
Apartment 의 프로퍼티인 tenant 에 weak 키워드를 사용하게되면 

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

아래 와 같이 서로 참조를 하게된다.

![](https://i.imgur.com/uSwCzk6.png)

하지만 이때 위의 예시와 달라진점이 있는데 Apartment 가 Person 을 약한 참조 하는것으로 변경되어있는걸 알수 있다.

이후 john 에 nil 을 할당해주면 deinit이 호출되게 된다.

![](https://i.imgur.com/nuZJQtl.png)

서로 순환 참조가 일어나지 않으므로 john 의 값이 nil 이되는순간 RC가 0이 되어 메모리에서 해제되게 된다.

이때 아파트에서 tenant 는 john 이었는데 john 이 nil 이되어버렸기 때문에 아파트의 tenant 는 자동적으로 nil 이된다.

ARC 는 참조하는 인스턴스가 해제되면 weak 참조 를 nil 로 바꿔 주는 기능 또한 가지고 있다!

**정리해보자**
1. weak 키워드를 사용하면 참조중이던 인스턴스가 nil 이될경우 weak 키워드를 사용한 값도 nil 로 변경되므로 항상 nil 값을 포함하는 옵셔널 로 선언되어있어야 한다.
2. weak 키워드를 사용하면 값이 nil 로 변경될수 있으므로 항상 var 로 선언 되어야 한다.

> 2개의 인스턴스중 수명이 더짧은 인스턴스에 약한참조를 사용한다.

설명이 어렵지만 쉽게 말해보자면 

아파트에는 입주자가 있을수도있고 없을수도 있다 그렇기에 옵셔널 값인데
아파트에 입주자가 있다가 입주자가 나갈수도있기 때문에 tenant 에 weak 키워드를 사용한것이다.

### 미소유 참조 (Unowned References)
weak 와 마찬가지로 RC 를 증가시키지 않는다.

**weak 와 Unowned 의 차이점이 있다면**
weak 은 2가지의 인스턴스중 수명이 더짧은경우 에 사용한다.
Unowned 은 반대로 다른 인스턴스와 수명이 동일하거나 더긴 경우 Unowned 를 사용한다.

Unowned 은 참조하는 인스턴스가 메모리에서 해제되어도 nil 로 만들어 주지 않는다.

그렇다면 var 일 필요도 옵셔널일 필요도 없겠네?????

쉽게 생각해보자면...1번뷰가 2번뷰를 참조해야하는 상황인데 2번 뷰가 1번뷰 보다 먼저 메모리에서 해제되면 안되는 경우..랄까?

내가 참조하는 상대방이 나보다 먼저 메모리에서 해제되지 않을경우 사용할수 있을것같다.

공식문서의 예제를 봐보자!
은행고객과 고객의 신용카드를 예시로든 예제 이다.

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}
```
고객은 신용카드가 없을수 있지만 신용 카드는 사용하는 고객이 없으면 안된다!
신용카드는 항상 고객과 연결되어 있어야 한다!

신용카드 입장에서는 고객이라는 값이 항상 있어야 하며 자기자신보다 먼저 메모리 해제가 일어나면 안된다..!

이럴때 unowned 를 사용한다!

```swift
var john: Customer?

john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```
현재 참조 상황은 아래와 같다

![](https://i.imgur.com/NHqieIa.png)

이후 john 에 nil 을 할당하게 될경우

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
// Prints "Card #1234567890123456 is being deinitialized"
```

![](https://i.imgur.com/aj4TeZH.png)

Customer instace 는 john 과 연결이 끊어지게 되고 
RC가 0 이 되면서 메모리에서 해제되게 된다.
왜 RC가 0임? 신용카드가 고객을 참조하고있는데??
위에서 설명했듯이 unowned 키워드를 사용하면 참조를 하고있어도 RC가 증가하지 않는다.

## 미소유 옵셔널 참조 (Unowned Optional References)
Unowned 키워드를 작성했는데 타입이 옵셔널 인경우를 말한다.
위에서 설명은 Unowned 을 사용할때는 자기자신보다 먼저메모리에서 해제되지 않을경우 사용한다고 했는데?? 
그렇기 때문에 Unowned 을 작성한 프로퍼티의 값이 nil 로 변경될일이 없었는데??

이건대체 왜 사용하는것일까..

일단 옵셔널이기때문에 nil 로 변경될수 있다는것을 알고있어야 겠다.
그렇다면 값이 변경될수 있기 때문에 var 로 선언해주어야 겠군..?
자동으로 메모리가 해제될경우 nil 로 변경 되지 않는것만 제외하면 weak 과 같은건가?

예제를 보도록하자!

대학교에서 학과 와 학과에 해당하는 수업에 관한 예제다.

```swift
class Department {
    var name: String
    var courses: [Course]
    init(name: String) {
        self.name = name
        self.courses = []
    }
}

class Course {
    var name: String
    unowned var department: Department
    unowned var nextCourse: Course?
    init(name: String, in department: Department) {
        self.name = name
        self.department = department
        self.nextCourse = nil
    }
}
```
학과는 어떤수업을 해야할지 정해두어야 하기때문에 수업을 배열 컬렉션으로 소유하고 있는 형태다.

수업은 어떤 학과의 수업인지 알고있어야 하며 수업이 사라지기전에 학과가 사라지는 경우는 없으니 학과 프로퍼티는 unowned 키워드를 사용하였다.

하지만 이제 문제가 되는것은 다음수업 이라는 프로퍼티인데 unowned 을 사용했음에도 옵셔널타입이다.

다음 수업이 있을지 없을지 모르기 때문이다.

```swift
let department = Department(name: "Horticulture")

let intro = Course(name: "Survey of Plants", in: department)
let intermediate = Course(name: "Growing Common Herbs", in: department)
let advanced = Course(name: "Caring for Tropical Plants", in: department)

intro.nextCourse = intermediate
intermediate.nextCourse = advanced
department.courses = [intro, intermediate, advanced]
```

학과는 원예 학과이며 수업 목록은 다음과 같다.
1. 식물 조사
2. 허브 재배
3. 열대 식물 돌보기

식물조사의 다음 수업은 허브재배
허브재배 다음 수업은 열대 식물 돌보기
열대식물 돌보기의 다음수업은 옵셔널 기본값인 nil 인 상태다.

![](https://i.imgur.com/YbKjt4f.png)

학과는 모든 수업을 강하게 참조하고 있는 형태이며
식물조사는 허브재배를 unowned? 형태로 소유하고 있다.

의문점은 다음수업이기때문에 weak 을 사용하는것이 맞지않나? 라는 의문이 든다. 
어떤 수업이 먼저 사라질지 알수 없기때문일까 추측 해본다.

unowned? 보다 weak 을 사용하는게 더 좋지 않을까?
unowned? 이 참조하고 있는 인스턴스가 메모리 해제되게 되면 자동으로 nil 로 변경되지 않아 일일히 어떤 것들을 nil 로 변경해주어야 하는지 찾아서 값을 변경해주어야 할것 같은데..

쉽게 말하면 학과에서 
department.courses 의 Element 중 무언가를 제거하게되면 
해당 Element 가 사용하고있는 unowned 프로퍼티의 값을 nil 로 변경해주어야 한다.

쓰다보니 생각했는데 혹시 모르게 unowned 키워드로 설계 했지만 나중에 먼저 메모리에서 해제될경우를 고려해서 옵셔널을 사용하는것일까??

예시를 생각해 보았다..이런 경우에 사용되지 않을까?
학과가 여러개 있고 수업도 여러개있을때
학과가 어떤 수업을 채택할지 정해지지 않은 경우 를 예시로 들어보자!

학과가 수업을 채택 한 후에는 수업보다 학과가 먼저 사라질일이 없다고 가정할때 unowned? 을 사용할수 있을것 같다.

처음엔 unowned? 프로퍼티의 값을 nil 로 설정한후 
초기화 하게되면 unowned? 프로퍼티에 인스턴스를 할당해준다!
그후로는 학과의 메모리가 먼저 해제될일이 없으니 이와 비슷한 형태로 사용할수 있을것 같다.

쉽게 말해 unowned? 프로퍼티 에 어떤값이 들어갈지 모를때, 하지만 값이 들어가게되면 자기자신보다 먼저 메모리 해제될일이 없을떄 사용하면될것 같다!
