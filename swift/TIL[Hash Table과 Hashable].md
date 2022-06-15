# Today I Learned.

## 학습내용
# Hash Table과 Hashable

## HashTable이란?
자료구조중 하나이며 Swift에서는 딕셔너리를 사용해 해시테이블을 사용할수있다!

딕셔너리는 Key-Value로 값을 저장한다.
딕셔너리가 해시 테이블로 구현되어 있기 때문에 해시 테이블도 Key-Value로 값을 저장한다.

해시테이블은 내부적으로 배열로 구현되어 있다.

Key-Value 값을 저장함수를 통해 배열에 저장 한다고 보면되겠다.

예시를 보자!

![](https://i.imgur.com/zB17htg.png)

위의 사진처럼 Key-Value 를 HashTable 에 저장하려고 한다!

이때 HashTable 에 저장하려고 할때 순서대로(0, 1...) 저장된다면 해시테이블이아니라 일반 배열이다.

해시테이블은 데이터가 순서를 지키지 않고 저장된다!

예시 사진은 저장하려는 값이 2개밖에없지만...무튼 해시테이블은 순서를 보장해주지 않는다!!

![](https://i.imgur.com/cEtw6BR.png)

해시테이블에 위의 사진처럼 저장되었다고 가정해보자!

malrang이라는 Key값으로 1번 index에 저장된 29라는 값을 얻어오게된다.

하지만 HashTable은 배열로 구성되어있다고했는데 Key값으로 어떻게 배열의 에 접근해 값을 가져올수 있을까?? index로만 접근할수 있는데??

그렇다면 Key값으로 index를 알수있게 해주는놈이 필요할테고 이러한 것을 가능하게 하는놈이 해시함수 라는 놈이다!

해시함수는 Key에 대한 산술 연산을 이용해 해시 주소값(HashTable 의 index)으로 만들어주는 놈이다!

![](https://i.imgur.com/8hSvnog.png)

정리해보자면 HashTable 이란 Key값을 해시함수를 이용해 해시주소값(HashTable 의 index)으로 바꾸고 해시주소값을 통해 HashTable에 접근하여 값을 가져오는 형태이다.

### HashTable의 특징과 장단점
**시간복잡도**

HashTable은 배열로 구성되어있지만 원하는 값을 찾기위해 0번 index부터 순회하여 찾는 배열의 O(n)과 다르게 Key값을 해시하여 바로 배열의 index에 접근하기 때문에 해시 테이블의 시간복잡도는 O(1) 이다.

하지만 충돌 문제를 해결하기위해 HashTable은 저장공간을 많이 사용하기 때문에 시간 복잡도와 공간 복잡도를 맞바꿨다 라고 표현한다고한다...

빠른 속도를 얻기위해 저장공간을 낭비한다..!

**HashTable의 장단점**

HashTable의 장점
- 데이터 저장/읽기 속도가 빠르다.
- Key에 대한 데이터의 중복 확인이 쉽다.

HashTable의 단점
- 일반적으로 저장공간이 많이 필요하다(충돌 문제를 해결하기 위해 저장공간을 많이 사용한다)
- 충돌이 발생 시 해결하기 위한 별도의 자료구조가 필요하다.

**HashTable의 용도**
- 저장, 검색, 삭제 등 탐색을 많이하는경우 사용시 유리하다.
- 캐쉬 구현할때 사용하면 좋다!

## Hashable

![](https://i.imgur.com/wOtXK8q.png)

정수 해시 값을 생성하기 위해 해시로 해시할 수 있는 유형입니다.?????

### Declaration
```swift
protocol Hashable : Equatable
```
Equatable 을 채택하는 놈이군요.

### Overview
프로토콜 을 준수하는 모든 유형을 Hashable세트 또는 사전 키로 사용할 수 있습니다. 표준 라이브러리의 많은 유형은 Hashable문자열, 정수, 부동 소수점 및 부울 값을 준수하며 기본적으로 집합도 해시 가능합니다. 선택적, 배열 및 범위와 같은 일부 다른 유형은 해당 유형 인수가 동일하게 구현될 때 자동으로 해시 가능하게 됩니다.

사용자 정의 유형도 해시 가능합니다. 연결된 값 없이 열거형을 정의하면 Hashable자동으로 적합성 을 얻고 메서드 Hashable를 구현하여 다른 사용자 지정 유형에 적합성을 추가할 수 있습니다. hash(into:)저장된 속성이 all 인 구조체의 Hashable경우 및 모든 관련 값이 있는 열거형 유형 Hashable의 경우 컴파일러는 의 구현을 hash(into:)자동으로 제공할 수 있습니다.

값을 해싱한다는 것은 해당 유형으로 표시되는 해시 함수에 필수 구성 요소를 제공하는 것을 의미 Hasher합니다. 필수 구성 요소는 의 형식 구현에 기여하는 구성 요소입니다 Equatable. 동일한 두 인스턴스 는 동일한 순서로 동일한 값을 Hasherin 에 공급해야 합니다.hash(into:)

### Conforming to the Hashable Protocol
사용자 정의 타입을 Set, Dictionary의 Key값으로 사용하려면 Hashable을 채택해야 한다.

Hashable은 Equatable을 채택하고 있으므로 Equatable의 요구 조건도 충족시켜주어야 한다.

Swift의 기본자료형(Int, String 등등)은 Equatable 과 마찬가지로 컴파일러에 의해 Hashable Protocl이 알아서 채택된다.

그렇기 때문에 기본 자료형을 딕셔너리에서 사용할경우 아무런 문제가 없다.
```swift
let dictionary: [String: Int] = ["malrang": 29]
```

딕셔너리의 Key값에 사용자 정의 타입을 넣을 경우 에러가 발생한다.
```swift
struct Malrang {
    let name: String
    var age: Int
}

let dictionary: [Malrang: Int]
// Type 'Malrang' does not conform to protocol 'Hashable'
```
Malrang의 name, age프로퍼티는 기본 자료형이지만 어떤 값으로 해시를 해야할지 모르기때문에 에러가 나는것이며 Hashable프로토콜을 준수하지 않았다 라는 에러가 발생하게된다.

Hashable프로토콜은 다음과 같이 생긴놈이다.

```swift
public protocol Hashable: Equatable {
    var hashValue: Int { get }
    func hash(into hasher: inout Hasher)
}
```

hashValue 프로퍼티는 더이상 사용하지 않는다고 공식문서에 나와있다.

![](https://i.imgur.com/3yWUz3c.png)

hash(into:) 함수는 필요할경우 직접 구현해주어야 한다.

## Hashable 을 채택/준수 해보자!
구조체, 클래스, 열거형 이 Hashable을 채택, 준수하는 방법이 모두 다르다..

각각 살펴보자!

### Struct
구조체의 경우에는 간단하다!
구조체 내부의 프로퍼티가 모두 기본 자료형이라면 Hashable을 채택하는 것만으로 추가구현없이 사용 가능하다!

```swift
struct Malrang: Hashable {
    let name: String
    var age: Int
}

let dictionary: [Malrang: Int] = [:]
```

### Class
class 의 경우에는 Hashable의 hash(into:)함수와 Equatable의 == 함수를 구현해주어야한다.

hash함수부터 구현해보자!

hasher파라미터의 combine 메서드를 이용해 구현하면 된다!

```swift
class Malrang {
    let name = "malrang"
    let age = 29
}
 
extension Malrang: Hashable {
    func hash(into hasher: inout Hasher) {
        hasher.combine(name)
        hasher.combine(age)
    }
}
 
let dictionary: [Malrang: Int] = [:]
```

hasher.combine이란 어떤 파라미터를 해시 할것인지를 넣어주는것이다.

**주의사항!**
특별한 경우가 아니면 combine에는 해당 타입의 모든 저장 프로퍼티를 전달한다.
combine 파라미터로 전달하는 프로퍼티는 반드시 Hashable을 준수하고 있어야 한다.

위의 예시에서 name, age 프로퍼티는 기본자료형이며 Hashable을 준수하고 있기 때문에 combine의 파라미터로 전달할수 있는거다!

그럼 다음으로 Equatable의 함수도 구현해보자!

```swift
class Malrang {
    let name = "malrang"
    let age = 29
}
 
extension Malrang: Hashable {
    static func == (lhs: Malrang, rhs: Malrang) -> Bool {
        return lhs.name == rhs.name && lhs.age == rhs.age
    }
    
    func hash(into hasher: inout Hasher) {
        hasher.combine(name)
        hasher.combine(age)
    }
}
 
let dictionary: [Malrang: Int] = [:]
```
요렇게 값을 비교할수있는 메서드를 구현해주어야한다.

### Enum
열거형은 연관값이 없는경우와 연관값이 있는경우로 나뉘게 된다!

1. 연관값이 없는경우
```swift
enum Camper {
    case malrang
}
 
let dictionary: [Camper: Int] = [:]
```
연관값이 없는경우는 Hashable을 채택하지않아도 된다!

2. 연관값이 있는경우
```swift
enum Camper: Hashable {
    case malrang(age: Int)
}
 
let dictionary: [Camper: Int] = [:]
```
Hashable을 채택해주고!!
연관값의 타입이 Hashable을 채택하고 있어야 한다.
위의 예시에서 age: Int 이기 때문에 Int는 Hashable을 채택하기 때문에 따로 구현해주지 않아도된다!

[Hashable](https://developer.apple.com/documentation/swift/hashable)
[항상 감사하는 소들이님](https://babbab2.tistory.com/149)
