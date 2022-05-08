# Today I Learned

# **학습내용**
참고한 문서
[공식문서 클로저](https://bbiguduk.gitbook.io/swift/language-guide-1/closures#the-sorted-method)
[개발자 소들이 블로그](https://babbab2.tistory.com/81?category=828998)

## Closure
>클로저란 익명 함수 즉 기존 사용하던 함수들도 클로저에 속한다.
>
>클로저 > 함수 (클로저가 더 큰 개념이다! 함수가 클로저의 부분 집합!)
그치만 편의상 클로저라고 말하는것은 여기서 이름없는 함수 를 뜻하게된다.
>
>이름 없는 함수라고도 할수 있으며 코드의 블럭 이라고도 표현할수 있겠다.
>
>Swift에서는 함수 패러다임 언어이기 때문에 함수는 1급 객체 이다.
그런고로 클로저도 1급 객체다!
>
>상수나 변수에 대입하는것도 가능하며 함수의 타입에 따라 파라미터로 사용할수도 있다.

### Closure 표현식
>이름 없는 함수 이기때문에 func 키워드를 작성하지 않으며 
클로저는 Head, Body 로 이루어져 있다.
```swift
{ (parameters)->return Type in 
    실행구문
}
```
>여기서 parameters 와 returnType in 부분이 Closure 의 Head 부분 되시겠다.
실행구문은 Closure 의 Body 부분이다.
>
>in 키워드 를 기준으로 나뉜다 생각하자.

### Parameter, ReturnType 이 없는 클로저
>파라미터와 리턴 타입이 없는 경우 다음과 같이 사용할수있다.
```swift
let closure = { () -> in
    print("Closure")
}
```
>여기서 함수처럼 반환타입이 없으면 생략 가능한것도 똑같으며 
파라미터가 없다면 파라미터도 생략이 가능하다!

```swift
let closure = { 
    print("Closure")
}
```

### Parameter, ReturnType 이 있는 클로저
>파라미터와 리턴 타입이 있는 경우 다음과 같이 사용할수있다.
```swift
let closure = { (name: String) -> String in
    return "Hi, \(name)"
}
```
>클로저의 파라미터에는 Argument Label 을 사용할수 없다.
오직 Parameter Name 으로만 작성이 가능하다!

### 클로저 호출
>클로저 를 호출할때는 다음과 같이 사용할수있다.
```swift
closure("malrang")
// "Hi, malrang"
```

### 함수의 파라미터 에 클로저를 넣어보자!
```swift
func someThing(closure: () -> ()) {
    closure()
}
```
>클로저를 파라미터로 전달받는 someThing 함수
>
>클로저 또는 함수를 넘겨줘도 되지만 호출할때 클로저 구현부를 작성해줘도 된다.
```swift
someThing(closure: { () -> () in
     print("Hi")
})
```

#### 함수의 반환타입을 클로저로!
```swift
func someThing() -> () -> () {
    return { () -> () in
        print("Hi Malrang!")
    }
}
```
>자 이게무슨 굉장한 코드냐...!
함수는 파라미터가없고 반환타입은 (closure) 다.
반환타입인 closure 는 다른 반환타입을 갖을수 있기때문에 위와같은 모습이 되게 된다.
>
>클로저를 반환했는데 클로저가 다른걸 반환시켰다,..! 라고 하면 이해가 편할까..

## Closure 축약 문법
>클로저의 표현식을 줄여 간단하게 표현할수있는 방법들이있다.
하지만 축약해서 사용하다보면 어떤의미를 가진 코드인지 불분명한 상황이 올수있으니 기본 Closure 문법이 숙달된후 프로젝트를 같이 진행하는 팀원 과 상의후 축약하여 사용하자!

### 1. Trailling Closure (트레일링 클로저)
>축약 문법중 하나이며 함수의 마지막 파라미터가 클로저 일때
파라미터 값 형식이 아니라 함수뒤에 붙여 작성하는 문법이다.
>
>클로저 하나만 파라미터로 받는 함수가 있을때 호출할때는 아래와 같이 사용했다.
>
>클로저, 클로저를 파라미터로 받는 함수 선언
```swift
let closure = { () -> () in
    print("고구마")
}

func doSomething(closure: () -> ()) {
    closure()
}
```
>클로저 를 파라미터로 받는 함수 호출
```swift
doSomething(closure: { () -> () in
    print("감자")
})

doSomething(closure: closure)
```
>이때 함수의 파라미터 안에 위치한 클로저를  Inline Closure(인라인 클로저) 라고한다! 
>
>위의 예시중 1번째 예시를 보면 클로저를 매개변수로 받지만 매개변수로 넣지 않고 클로저를 작성하여 넣어준 예시다!
1번째 예시 코드의 마지막을 보면 }) 이런 식으로 되어있어 헷갈리는데 이를 해결하기 위한 방법이 Trailling Closure 다.

1. 파라미터가 클로저 하나일 경우
```swift
doSomething() { () -> () in
        print("감자")
}
```
>위의 예시 처럼 파라미터 뒤에 중괄호 를 작성하는 형태로 작성할수 있다.
>
>이때 파라미터가 클로저 하나일 경우엔 파라미터 를 나타내는 () 소괄호 를 생략할수 있다.

```swift
doSomething { () -> () in
        print("감자")
}
```

2. 파라미터가 클로저 두개인 경우
```swift
func 농작물(감자: () -> (), 고구마: () -> ()) {
    감자()
    고구마()
}
```
**Inline Closure 의 경우**
>파라미터인 () 소괄호 내부에 작성해주어야 한다.
```swift
농작물(감자: { () -> () in
    print("감자")
}, 고구마: {() -> () in
    print("고구마")
})
```
**Trailling Closure**
>파라미터 2개가 클로저 이지만 마지막 파라미터가 클로저일때는 trailing Closure 를 사용할수 있으니 마지막 파라미터인 클로저를 작성하게되면 아래와 같은 형태가 된다.
```swift
농작물(감자: { () -> () in
    print("감자")
}) { () -> () in
    print("고구마")
}
```
>파라미터 두개중 하나는 () 내부에 또다른 하나는 () 밖에 {} 내부에 위치하는 모습이다.

## 파라미터와 리턴 타입을 생략 하자!
>위의 예시들을보면 게속 사용되었던 () -> () 라는 키워드
클로저의 파라미터와 리턴 타입을 나타내는데 파라미터의 타입과 리턴타입이 동일 하다면 생략이 가능하다!

```swift
농작물(감자: { in
    print("감자")
}) { in
    print("고구마")
}
```

>여기서 또 파라미터 가 없는경우 in 키워드 는 제거해주어야 한다!

```swift
농작물(감자: { 
    print("감자")
}) { 
    print("고구마")
}
```

>매개변수가 있고 반환 값이 있는 클로저를 예시로 들어보자!

**선언**
```swift
func 농작물(closure: (String, String) -> String) {
    closure()
}

```
**호출**
```swift
농작물(closure: { (작물1: String, 작물2: String) -> String in
    return 작물1 + 작물2
})

```
>위의 코드중 호출 상황을 보면 위에서 설명햇듯이 파라미터 타입과 반환 값의 타입이 일치하면 생략이 가능하다고 했다.
생략하면 아래와 같은 모습이된다.
```swift
농작물(closure: { (작물1, 작물2) in
    return 작물1 + 작물2
})
```
>여기서 또 줄여 사용할수있다!

### Shortand Argument Names($0, $1)

>Parameter Name은 Shortand Argument Names으로 대체하고, 이 경우 Parameter Name과 in 키워드를 삭제한다.
>
>이게 무슨 말이냐 
>
>설명에서 Parameter Name 은 작물1, 작물2 를 뜻한다 얘내를 Shortand Argument Names 로 대채하고 파라미터 와 in 키워드를 삭제할수 있단다.
>
>Shortand Argument Names 는 $ 와 index 를 이용해 표기하는것으로 파라미터 혹은 배열의 Element 에 접근할수 있게 된다. 
>
>사용법은 이렇다.

```swift
작물1 = $0
작물2 = $1
```
>index 를 이용해 표기 한다고 했으니 파라미터의 첫번째는 0번인덱스 즉 0 이된다.
>
>위의 표기법으로 변경하게되면

```swift
농작물(closure: { ($0, $1) in
    return $0 + $1
})
```
>과 같이 줄여 표현할수 있으며 
위에 설명한 것처럼 
Parameter Name 이 Shortand Argument Names 으로 대체될 경우 in 키워드를 삭제할수 있게되므로

```swift 
농작물(closure: {
    return $0 + $1
})
```
>이러한 형태가 된다.

### 클로저의 내부가 단일 return 구문 일경우! 제거가능!
>하지만 여기서 또 클로저 내부에 return 구문 하나만 남은경우 return 키워드도 생략이 가능해진다!

```swift 
농작물(closure: {
     $0 + $1
})
```
>와 같이 표기할수 있게 된다!
>
>굉장히 많이 짧아졌다! 여기서 파라미터는 클로저 하나이기때문에 아까 얘기한 트레일링 클로저를 적용할수 있게되며 적용한 모습은 다음과 같다!

```swift
농작물 { $0 + $1 }
```
>크으..! 그렇지만 클로저의 내부가 복잡할경우엔 이렇게 줄여 사용하게되면 의미를 알수없게되어 코드를 이해하는데 시간이 오래걸릴수 있으니 주의 하자!

## Closure 캡처값 (Capturing Values)
>클로저는 상수와 변수를 캡쳐 할수 있다.
클로저는 캡처를 하게 되면 상수와 변수가 접근할 수 없게 되더라도 클로저는 해당 상수 및 변수의 값을 참조 하고 수정할 수 있다.
swift 에서 가장 쉽게 값을 캡처 하는 방법은 중첩 함수를 사용하는 것이다.
중첩함수는 자신을 둘러싼 함수의 매개 변수도 캡처할 수 있고 정의된 상수나 변수도 캡처할 수 있다.

**중첩 함수 예시**
```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int { 
    var runningTotal = 0 
    func incrementer() -> Int { 
        runningTotal += amount 
        return runningTotal 
    } 
    return incrementer }
```
>함수 내부에 함수가 위치한 구조다.
>
>makeIncrementer() 함수는 파라미터 타입은 Int 이며
반환값은 클로저를 반환하며 클로저의 반환 값은 Int 타입이다.
>
>중첩함수 incrementer() 내부 에서 외부함수 makeIncrementer() 의 파라미터 amount 와 함수내에서 정의된 변수 runningTotal 를 캡처 하여 사용하는것을 볼수 있다.
>incrementer() 함수에는 파라미터인 상수와 runningTotal 변수가 없지만 캡처하여 사용할수 있는것이다.

**호출 예시**
```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
incrementByTen() // returns a value of 10 
incrementByTen() // returns a value of 20 
incrementByTen() // returns a value of 30
```
>호출 할때마다 incrementByTen 의 값이 10씩 증가 하게된다.
>
>`let incrementByTen = makeIncrementer(forIncrement: 10)` 구문에서는 값이 증가되지 않고 `runningTotal = 0`인 상태이다.
>
>호출하게되면 그제서야 중첩함수 incrementer 가 실행된다고 할수 있다.
>
>makeIncrementer() 함수 내부의 값이 사라지지 않고 게속 유지되고 있음을 알수 있다.

### 클로저는 참조 타입 (Closures Are Reference Types)

>위의 예시 에서본 incrementByTen 은 상수로 선언된 값이지만 게속 내부의 값이 바뀌는것을 볼수 있다. 
이유는 함수와 클로저는 참조 타입이기 때문이며 같은 클래스를 참조하여 값을 변경하는것과 같다.
>
>함수나 클로저를 상수나 변수에 할당하게 되면 상수나 변수에 복사되는것이 아닌 메모리 주소만 참조하게 되고 다른 상수나 변수에 incrementByTen 을 할당하게 되면 아래와 같은 일이 일어나게 된다.

![](https://i.imgur.com/Fkp74gj.png)

>incrementByTen 을 상수 some 에 incrementByTen 호출된 값을 대입할경우 some 의 값은 20 이된다(incrementByTen 이 2번 호출 되었기 때문에)
>
>그후 Some 를 호출하지 않고 다른 상수,변수에 대입하게되면 이번엔 참조가 아닌 값을 넘겨주게 된다.
하지만 some 을 호출하게 되면 some과 incrementByTen 은 같은것을 참조 하고 있기 떄문에 some, incrementByTen 2개 모두 30 의 값을 가지게 되겠다.

## @autoClosure
>파라미터로 전달된 일반 구문 또는 함수를 클로저로 래핑하는것.

**표현식**
```swift
func doSomething(closure: @autoclosure () -> ()) {
}
```
>파라미터인 클로저 는 실제 클로저를 전달받지 않지만 클로저 처럼 사용이 가능하다.

**오토클로저가 적용된 함수 정의 및 호출**
```swift

func doSomething(closure: @autoclosure () -> ()) {
    closure()
}

doSomething(closure: 1 > 2)
```
>주의점은 autoClosure 를 사용할경우 파라미터가 없어야 한다.
리턴타입은 상관 없다.
>
**사용예시**
![](https://i.imgur.com/d6mxfkm.png)

>위에 설명한대로 오토클로저를 사용한후 오토클로저의 반환값을 Int 타입을 반환하도록 했다.
그후 함수의 반환타입을 Int 타입으로 설정한 예시이다.
>
>함수 호출시 클로저에 Int 값을 넣었더니 해당 값이 반환되는것을 확인 할수 있다.

### @autoclosure 특징
>일반 구문은 작성되자마자 실행된다.
하지만 autoclosure 는 함수내에서 클로저를 실행할 때까지 구문이 실행되지 않는다.
함수가 실행될 시점에 구문을 클로저로 래핑해주어야 하기 때문이다.
>
>그렇기에 autoclosure 의 특징은 지연되어 실행한다는 특징을 가지고 있다.

## @escaping
>직역하면 탈출 이라는 뜻이다.
지금 까지 사용하던 클로저는 모두 non-escaping Closure 다.

**non-escaping Closure특징**
>함수 내부에서 직접 실행하기 위해서만 사용한다.
따라서 파라미터로 받은 클로저를 변수나 상수에 대입할수 없고
중첩함수에서 클로저를 사용할 경우, 중첩함수를 리턴할수 없다.
함수의 실행 흐름을 탈출하지않아 함수가 종료되기 전에 무조건 실행되어야 한다.
>
>파라미터로 받은 클로저를 함수내부의 변수나 상수에 대입할수 없다.

![](https://i.imgur.com/6bkRHu7.png)

>함수가 종료되고 나서 함수 내부의 클로저가 실행될수 없다.
>
>아직 이부분은 함수가 종료되고 함수내부의 구문을 실행시키는 방법을 몰라 예시를 작성할수 없다.
>
>하지만 defer 구문은 사용가능한것으로 보아 defer 는 함수가 종료되고 defer 구문을 실행시키는것이 아닌 함수 실행 순서만 변경되는것 같다. 
>
![](https://i.imgur.com/1Q51Q0Z.png)

>중첩함수 내부에서 매개변수로 받은 클로저를 사용할경우 중첩함수를 리턴할수 없다.

![](https://i.imgur.com/ku0lwvR.png)

>모든 에러의 원인은 non-escaping Closure 의 주변 값 캡쳐 방식 때문이다.

**@escaping 클로저 사용이유**
>함수가 끝난 후에도 클로저를 실행하거나
중첩함수 에서 실행 후 중첩 함수를 리턴하고 싶은경우 
파라미터로 받은 클로저를 변수나 상수에 할당 하고 싶은경우
사용하는것이 @escaping 클로저다.

**표현식**

![](https://i.imgur.com/FHcB5jV.png)

> escaping 키워드를 클로저의 파라미터 타입앞에 작성하면 된다.
파라미터로 전달된 클로저를 함수 내부의 상수에 할당한 모습.

![](https://i.imgur.com/mIlN47b.png)

>이것 말고도 함수가 종료된 후에도 클로저가 실행될수 있다.
>
>하지만 이떄 메모리 관련하여 문제가 될수있다.
함수가 종료된 후에 함수내부의 값을 사용하기 때문이다.
