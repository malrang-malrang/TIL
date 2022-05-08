# Today I Learned

## **학습내용**
### **Protocol**
> A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality.

- 프로토콜은 메서드나 프로퍼티나 다른 요구사항들에 대한 청사진은 정의 합니다.
- 사용자 타입은 해당 프로토콜을 채택한후, 프로토콜의 요구사항을 구현하고 실제 동작도 그때 구현해서 사용해야 합니다.
- 즉 프로토콜을 채택했다면 해당 프로토콜이 요구하는 메서드,프로퍼티를 필수로 구현해야하며 프로토콜을 채택한 사용자 정의 타입은 저 프로퍼타입을 채택했으니 이러이러한 기능을 필수로 가지고 있겠구나 라고 해석할수 있게 해줍니다.

예시
```swift
protocol Something {
    var some: Int { get }
    func printSome()
}

class AnySome: Something {
    var some: Int
    
    init(some: Int) {
        self.some = some
    }
    
    func printSome() {
    
    }
}
```
위의 예시처럼 Something라는 프로토콜을 채택할경우 해당프로토콜 내부에 있는 프로퍼티,메서드를 채택한 class에서 구현해주어야 하며 구현하지 않게되면 에러를 발생시킵니다

- 프로토콜 내부의 메서드,프로퍼티 의 바디는 비워져있어야 하며 해당 부분을 프로토콜을 채택한 class에서 정의해주어야 한다.


### **CaseIterable**
> When using a CaseIterable type, you can access a collection of all of the type’s cases by using the type’s allCases
 property.
 
 enum(열거형) 에서 caseIterable 프로토콜을 채택 할경우 allCases 타입 프로퍼티를 사용할수 있게 해준다.
 
 예시
 ```swift
enum Something: CaseIterable {
    case a, b, c
}

let someCharacter = Something.allCases.randomElement()
```
위의 예시 처럼 enum 을 컬렉션 타입처럼 활용할수 있게 된다.

작동 방식은 프로토콜을 채택하였기 때문에 CaseIterable 의 해당 구현부를 enum에 구현시켜두어야 하지만 구현하지 않는다면 컴파일러가 자동으로 구현해주어 따로 구현하지 않아도 됩니다.

즉 따로 구현부를 작성하지 않아도 allCases를 사용할수 있게 해줍니다.

### **삼항 연산자**
삼항 조건 연산자 (ternary conditional operator) 는 question ? answer1 : answer2 형태의 3가지 부분으로 이루어진 특별한 연산자입니다. 
question 이 참 또는 거짓인지에 따라 2개의 표현식 중 하나를 나타내는 식입니다. question 이 참이라면 answer1 을 반환하고 반대면 answer2 를 반환합니다.

사용예시
```swift
question ? answer1: answer2
```

삼항 연산자를 풀어서 사용하게 되면
```swift
if question {
    answer1
} else {
    answer2
}
```
위와 같은 모습으로 풀어 사용할수 있다.
조건이 참,거짓 으로 분기를 나누어 실행시켜줄수 있게된다.

기능상에도 큰 차이없이 코드를 줄여주는 효과를 얻을수있다. 
하지만 가독성과 디버깅 부분에 문제가 생길수 있으니 고민해보고 사용하도록 하자

위의 if문 같은경우 디버깅시에 각 부분을 breakpoint로 지정해 프로그램을 정지하여 테스트해줄수 있으나 삼항 연산자를 사용했을경우 분기점에 breakpoint를 걸어줄수 없기 때문에 문제가 생겼을경우 디버깅하기 어려워 질수 있으며 가독성도 한번에 이해하기 쉬운 if문보다 떨어지게된다.

### **Struct, Enum, Class**
참고자료
[Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)

기본적으로 구조체를 사용하고 Class의 기능이필요할때(상속, Objective-C 호환성이 필요할때, Identity가 필요할때 등)만 Class를 사용하라고 권장하고 있다.

각각 의 차이점과 공통점을 나열해본다.
* enum
    * 연관된 값들의 집합으로 공통된 타입으로 정의한다.
    * extension사용 가능.
    * 상속 불가.
    * 저장프로퍼티 사용불가.
    * 연산프로퍼티 사용가능.
    * RawValue 를 통해 원시값 지정가능.
    * protocol 사용가능.

* class(클래스)
    * 단일상속 가능.
    * heap영역에 저장, 상대적으로 느리다.
    * 참조 타입이다.
    * deinitializer 사용가능.( 클래스의 인스턴스가 메모리에서 해제되는 시점에 호출되는 함수)

* struct(구조체)
    * 상속 불가.
    * stack영역에 저장, 상대적으로 빠르다.
    * 멀티 쓰레드 환경에서 안전하다.
    * 값 타입이다.
    * swift 뼈대 대부분 구조체로 구성되어 있다.

* struct, class 공통점
    * extension사용가능(구조체, 클래스, 열거형, 프로토콜 타입에 새로운 기능을 추가가능)
    * protocol사용가능(특정 역할을 수행하기 위한 메서드, 프로퍼티, 기타요구사항등의 청사진)
    * initalizer 정의가능.

값타입과 참조타입, 뜬구름처럼 어떤느낌인지 감은 잡았지만 한문장으로 설명할수 없다 좀더 고민해보고 공부해보도록하자.
heap과 stack 에 대해서도 마찬가지!

### **In-Out파라미터**
#### 특징
- 함수의 파라미터는 기본적으로 상수 여서 변경이 불가능하다. 이때 In-Out 파라미터를 사용하면 매개변수로 전달된값을 변경할수있습니다.

#### In-Out 파라미터 작동 방식
- 함수가 호출되면 인수값이 복사됩니다.
- 함수 본문에서 복사본이 수정됩니다.
- 함수의 라이프사이클이 끝나면 복사본의 값이 원래 인수에 할당 됩니다.

In-Out 파라미터를 사용할때 메모리 의 안정성때문에 에러가 날수있으니 조심히 사용해주어야 한다!

최적화를 위해 인수가 물리적 주소에 저장된 값 인경우 함수 본문 내부와 외부에서 동일한 메모리 위치를 사용합니다.

예시
```swift
var stepSize = 1

func increment(_ number: inout Int) {
    number += stepSize
}

increment(&stepSize)
```
위의 예시에서 보면 stepSize라는 변수를 increment라는 함수에서 In-Out파라미터로 사용 하고있다. 함수내부에서는 In-Out파라미터와 외부에있는 stepSize를 같이 사용해주고있다.
이때 number와stepSize 모두 같은 메모리위치를 사용해주고있으므로 메모리 충돌 에러가 일어나게된다.

확인후 사용하도록 하자!

