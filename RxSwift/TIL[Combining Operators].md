# Today I Learned.

## 학습내용
# RxSwift Combining Operators

# Combining Operators
- 다양한 방법으로 sequence들을 모으고, 각각의 sequence내의 데이터들을 병합하는 방법을 배워보자.

- observable로 작업할 때 가장 중요하게 확인해야 할 것은 observer가 초기값을 받는지 여부다.

## 앞에붙이기
### 1. startWith()
![](https://i.imgur.com/QhfLEKe.png)

- 요녀석은 초기값을 갖는 옵저버블 이라고 할수있겠다.
- 이벤트가 발생되어 next로 방출될때 초기값을 방출한후 전달된 이벤트들을 방출할수 있게된다.

    네트워크 연셜 상태 같이 현재 상태가 필요한 상황에 요녀석을 사용해 현재 상태를 초기값으로 붙여줄수 있겠다.

    **예제코드**
    ```swift
     let numbers = Observable.of(2, 3, 4)
    
         let observable = numbers.startWith(1)
         observable.subscribe(onNext: {
             print($0)
         })
    ```
    요롷게 startWith(1)하면 1, 2, 3, 4 순서대로 출력됨니다!
    numbers에 초기값을 1로넣어주었기 때문이지요!
     
- 좀더 자세히 설명하자면 startWith()는 Observable sequence를 이어붙이는거다. startWith() 의 위치와 상관없이 맨 앞부분에 sequence를 이어붙이기 때문에 추후 어떤 업데이트가 있더라도 초기값을 즉시 얻을수 있는 Oberverble을 보장한다.

### 2. Observable.concat(_:) 타입오퍼레이터
![](https://i.imgur.com/Vpzapmu.png)

- 방금 startWith 예제로 구현한 것은 하나의 값을 갖는 sequence를 다른 sequence에 연결한 것이다.
- Observable.concat(_:)을 통해서는 두개의 sequence를 묶을 수 있다.
    **예제코드**
    ```swift
    let first = Observable.of(1, 2, 3)
     let second = Observable.of(4, 5, 6)

     let observable = Observable.concat([first, second])

     observable.subscribe(onNext: {
         print($0)
     })
    ```
    예상한대로 observable2개를 붙였기때문에(하나의 sequence가됨)1, 2, 3, 4, 5, 6 순서대로 방출된다!
    
- 첫 번째 콜렉션의 sequence의 각 요소들이 완료될 때까지 구독하고, 이어서 다음 sequence를 같은 방법으로 구독한다. 이러한 과정은 콜렉션의 모든 observable 항목이 사용될 때까지 반복된다.
- 만약에 내부의 observable의 어떤 부분에서 에러가 방출되면, concat된 observable도 에러를 방출하며 완전 종료된다.

### 3. concat(_:) 인스턴스오퍼레이터
- 요녀석도 크게 다를건 없다.
    **예제코드**
    ```swift
    let germanCities = Observable.of("Berlin", "Münich", "Frankfurt")
     let spanishCities = Observable.of("Madrid", "Barcelona", "Valencia")

     let observable = germanCities.concat(spanishCities)
     observable.subscribe(onNext: { print($0) })
    ```
    요녀석은 인스턴스 오퍼레이터 이기때문에 로드된 인스턴스에서 사용할수있으며 기존 Observable이 완료될때까지 기다린후 다음 Observable을 구독한다.

- Observable의 결합은 반드시 두 Observable의 요소들이 같은 타입일 때 가능하다. 다른 타입의 Observable을 합치려고하면 컴파일 에러가 발생된다.

### 4. concatMap(_:)
- flatMap과 밀접한 관련이 있다.
- flatMap을 통과하면 Observable sequence가 구독을 위해 리턴되고, 방출된 observable들은 합쳐지게 된다.
- concatMap은 각각의 sequence가 다음 sequence가 구독되기 전에 합쳐진다는 것을 보증한다.

    **예제코드**
    ```swift
    let sequences = ["Germany": Observable.of("Berlin", "Münich", "Frankfurt"),
                      "Spain": Observable.of("Madrid", "Barcelona", "Valencia")]

     let observable = Observable.of("Germany", "Spain")
         .concatMap({ country in
             sequences[country] ?? .empty() })

     _ = observable.subscribe(onNext: {
         print($0)
     })
    ```
    뭔가 하고 한참봤다.
    1. 옵저버블 딕셔너리가 있다.
    2. "Germany", "Spain"를 방출하는 옵저버블이 있는데 내부에서 방출된 값을 옵저버블 딕셔너리의 키값과 매핑한다
    3. 딕셔너리에 저장된 value들을 방출하게 한다.
    4. 결과적으로 sequences에 저장된 두개의 옵저버블을 하나로 합친다.

## 합치기
### 1. merge()
![](https://i.imgur.com/xlilmlZ.png)

- RxSwift에는 sequence들을 합치는 다양한 방법들이 있다. 시작하기에 가장 쉬운 방법은 merge다.
- 위의 이미지만 봐도 2개의 sequence를 합친모습을 볼수있는데 이때 중요한것은 방출되는 순서도 적용된다는 점이다.

    **에제코드**
    ```swift
     let left = PublishSubject<String>()
     let right = PublishSubject<String>()

     let source = Observable.of(left.asObservable(), right.asObservable())

     let observable = source.merge()
     let disposable = observable.subscribe(onNext: {
         print($0)
     })

     var leftValues = ["Berlin", "Münich", "Frankfurt"]
     var rightValues = ["Madrid", "Barcelona", "Valencia"]

     repeat {
         if arc4random_uniform(2) == 0 {
             if !leftValues.isEmpty {
                 left.onNext("Left: " + leftValues.removeFirst())
             }
         } else if !rightValues.isEmpty {
             right.onNext("Right :" + rightValues.removeFirst())
         }
     } while !leftValues.isEmpty || !rightValues.isEmpty

     // 5
     disposable.dispose()
    ```
    두 개의 PublishSubject 인스턴스를 준비한다.
    각각의 PublishSubject를 asObservable()을 이용해서 Observable로 만들고, 이 observable들 타입으로 갖는 source라는 이름의 observable을 만든다.
    두 개의 observable을 합쳐서 구독하도록 한다.
    각각의 observable에서 랜덤으로 값을 뽑는 로직을 작성한다. leftValue와 rightValue에서 모든 값을 출력한 후 종료될 것이다.
    아직 Subject는 완료되지 않았기 때문에, dispose()를 호출하여 메모리 누수가 일어나지 않도록 한다.
    출력값은 랜덤이므로 구동할 때마다 다르게 나타날 것이다.

- merge() observabled은 각각의 요소들이 도착하는대로 받아서 방출한다.
- merge()는 source sequence와 모든 내부 sequence들이 완료되었을 때 끝난다.
- 내부 sequence 들은 서로 아무런 관계가 없다.
- 만약 어떤 sequence라도 에러를 방출하면 merge()는 즉시 에러를 방출하고 종료된다.
- 위의 코드를 다시 확인해보면, merge()는 observable들 요소로 가지고 방출하는 source observable을 취하는 것을 알 수 있다. 즉, merge()에 많은 양의 sequence들을 보낼 수 있다는 뜻이다.

### 2. merge(maxConcurrent:)
- 합칠 수 있는 sequence의 수를 제한하기 위해서 merge(maxConcurrent:)를 사용할 수 있다.
- limit에 도달한 이후에 들어오는 observable을 대기열에 넣는다. 그리고 현재 sequence 중 하나가 완료되자마자 구독을 시작한다.
- 이러한 제한 메소드를 merge()보다 덜 사용하게 될 가능성이 크다. 하지만 적절한 용도가 있다는 것을 항상 기억해두자. 네트워크 요청이 많아질 때 리소스를 제한하거나 연결 수를 제한하기 위해 merge(maxConcurrent:) 메소드를 쓸 수 있다.

## 요소 결합하기
### 1. combineLatest(::resultSelector:)
![](https://i.imgur.com/gcPNMsQ.png)

- sequence 두개 묶어서 두개의 옵저버블이 방출하는값도 묶어서 사용할수있다.
- 묶여질 두개의 옵저버블의 타입은 같지 않아도된다.
- 여러 TextField를 한번에 관찰하고 값을 결합하거나 여러 소스들의 상태들을 보는 것과 같은 app이 있다.

    **예제코드**
    ```swift
    let left = PublishSubject<String>()
     let right = PublishSubject<String>()

     let observable = Observable.combineLatest(left, right, resultSelector: { lastLeft, lastRight in
         "\(lastLeft) \(lastRight)"
     })

     let disposable = observable.subscribe(onNext: {
         print($0)
     })

     print("> Sending a value to Left")
     left.onNext("Hello,")
     print("> Sending a value to Right")
     right.onNext("world")
     print("> Sending another value to Right")
     right.onNext("RxSwift")
     print("> Sending another value to Left")
     left.onNext("Have a good day,")

     disposable.dispose()

     /* Prints:
      > Sending a value to Left
      > Sending a value to Right
      Hello, world
      > Sending another value to Right
      Hello, RxSwift
      > Sending another value to Left
      Have a good day, RxSwift
     */
    ```
    왼쪽에 "Hello"라는 값을 전달했을땐 방출되지 않는다.
    오른쪽에 "world"라는 값을 보냈을때 비로소 합쳐진 "Hello world"라는 문구가 출력된다.
    두개를 묶지 못하면 방출되지 않는다는뜻이다.
    이후 오른쪽에 "RxSwift"라는 값을 전달했다.
    왼쪽에는 전달된 값은 없지만 가장 최근에 전달된 값인 "Hello"와 묶여 "Hello RxSwift"가 방출되는것을 볼수있다.
    가장 최근에 전달된 값을 묶어서 방출하는걸 알수있다.
- 일반적인 패턴은 값을 튜플에 결합한 다음 체인 아래로 전달하는 것이다. 예를 들어 값을 결합한 다음 필터를 호출하는 등의 작업을 할 수 있다.

### 2. combineLatest(,,resultSelector:)
- combineLatest 계열에는 다양한 연산자들이 있다. 이들은 2개부터 8개까지의 observable sequence를 파라미터로 가진다. 앞서 언급한대로, sequence 요소의 타입이 같을 필요는 없다.

    ```swift
    let choice: Observable<DateFormatter.Style> = Observable.of(.short, .long)
     let dates = Observable.of(Date())

     let observable = Observable.combineLatest(choice, dates, resultSelector: { (format, when) -> String in
         let formatter = DateFormatter()
         formatter.dateStyle = format
         return formatter.string(from: when)
     })

     observable.subscribe(onNext: { print($0) })    
    ```
    위의 예제는 유저가 날짜를 표기하는 설정을 바꿀때마다 자동적으로 화면을 업데이트 시켜준다.
    
### 3. combineLatest([],resultSelector:)
- array내의 최종 값들을 결합하는 형태도 있다.
- 맨처음 예제에서 combineLatest(_:_:resultSelector:) 를 사용해 수정할수도 있다.

    **예제코드**
    ```swift
    let left = PublishSubject<String>()
     let right = PublishSubject<String>()

     let observable = Observable.combineLatest([left, right]) { strings in
         strings.joined(separator: " ")
     }


     let disposable = observable.subscribe(onNext: {
         print($0)
     })
    ```
    요렇게 말이다!
    
### 4. zip
![](https://i.imgur.com/xeNZqBN.png)

 - combineLatest 랑 조금다르게 순서를 기억해 묶어준다!(가장 최근값과 묶어주지 않는다!)
   
   **예제코드**
    ```swift
     enum Weather {
         case cloudy
         case sunny
     }
    
     let left:Observable<Weather> = Observable.of(.sunny, .cloudy, .cloudy, .sunny)
     let right = Observable.of("Lisbon", "Copenhagen", "London", "Madrid", "Vienna")
    
     let observable = Observable.zip(left, right, resultSelector: { (weather, city) in
         return "It's \(weather) in \(city)"
     })
    
     observable.subscribe(onNext: {
         print($0)
     })
    /* Prints:
      It's sunny in Lisbon
      It's cloudy in Copenhagen
      It's cloudy in London
      It's sunny in Madrid
      */
    ```
    Weather enum을 작성하고, 두개의 Observable을 만든다.
    zip(_:_:resultSelector:)를 이용하여 두개의 Observable을 병합한다.
    상기 코드에서 Vienna가 출력되지 않은 것을 알 수 있다. 왜일까?
    이들은 일련의 observable이 새 값을 각자 방출할 때까지 기다리다가, 둘 중 하나의 observable이라도 완료되면, zip 역시 완료된다.
    더 긴 observable이 남아있어도 기다리지 않는 것이다. 이렇게 sequence에 따라 단계별로 작동하는 방법을 가르켜 indexed sequencing 이라고 한다.

- 각각 방출된 순서대로 묶여지는것을 확인할수있다.
- combineLatest처럼 2 ~ 8개의 observable에 대한 변형과 조합을 제공한다.

## Triggers
- 여러개의 observable을 한번에 받는 경우가 있을 것이다. 
- 이럴 때 다른 observable들로부터 데이터를 받는 동안 어떤 observable은 단순히 방아쇠 역할을 할 수 있다.

### 1. withLatestFrom(_:)
- 방아쇠!

    **예제코드**
    ```swift
    let button = PublishSubject<Void>()
     let textField = PublishSubject<String>()

     let observable = button.withLatestFrom(textField)
     _ = observable.subscribe(onNext: { print($0) })

     textField.onNext("Par")
     textField.onNext("Pari")
     textField.onNext("Paris")
     button.onNext(())
     button.onNext(())
    /* Prints:
    paris
    paris
      */
    ```
    button과 textField라는 두개의 PublishSubject를 만든다.
    botton에 대해 withLatestFrom(textField)를 호출한 뒤, 구독을 시작한다.
    button에 새 이벤트가 추가되기 직전에 textField가 추가된 최신 값인 Paris가 출력된다. 그 전의 값들은 무시된다.

### 2. sample(_:)
- withLatestFrom(_:)과 거의 똑같이 작동하지만, 한 번만 방출한다. 즉 여러번 새로운 이벤트를 통해 방아쇠 당기기를 해도 한번만 출력되는 것.

    **예제코드**
    ```swift
    let button = PublishSubject<Void>()
    let textField = PublishSubject<String>()

    let observable = textField.sample(button)
    _ = observable.subscribe(onNext: { print($0) })

    textField.onNext("Par")
    textField.onNext("Pari")
    textField.onNext("Paris")
    button.onNext(())
    button.onNext(())
    /* Prints:
    paris
      */
    ```
    위의 예시코드에서 중간부분만 변경해두었다.
    요러면 paris가 한번만 출력된다.
    
- withLatestFrom(_:)을 가지고 sample(_:)처럼 작동하게 하려면 distinctUntilChanged()와 함께 사용하면 된다. 아래의 코드처럼 작성하면 Paris가 한번 출력된다.

    ```swift
    let observable = button.withLatestFrom(textField)
     _ = observable
         .distinctUntilChanged()
         .subscribe(onNext: { print($0) })
    ```
- withLatestFrom(_:)은 데이터 observable을 파라미터로 받고, sample(_:)은 trigger observable을 파라미터로 받는다. 실수하기 쉬운 부분이니 주의할 것

## Switches


### 1. amb(_:)
- amb(_:)에서 amb는 ambiguous(모호한) 이라 생각하면 된다.
- 두가지 sequence의 이벤트 중 어떤 것을 구독할지 선택할 수 있게 한다.

    **예제코드**
    ```swift
    let left = PublishSubject<String>()
     let right = PublishSubject<String>()

     let observable = left.amb(right)
     let disposable = observable.subscribe(onNext: { value in
         print(value)
     })

     left.onNext("Lisbon")
     right.onNext("Copenhagen")
     left.onNext("London")
     left.onNext("Madrid")
     right.onNext("Vienna")

     disposable.dispose()
    ```
    left와 right를 사이에서 모호하게 작동할 observable을 만든다.
    두 개의 observable에 모두 데이터를 보낸다.

    amb(_:) 연산자는 left, right 두 개 모두의 observable을 구독한다.
    그리고 두 개중 어떤 것이든 요소를 모두 방출하는 것을 기다리다가 하나가 방출을 시작하면 나머지에 대해서는 구독을 중단한다.
    그리고 처음 작동한 observable에 대해서만 요소들을 늘어놓는다.
    
    처음에는 어떤 sequence에 관심이 있는지 알 수 없기 때문에, 일단 시작하는 것을 보고 결정하는 것이다.

### 2. switchLatest()
- observable로 들어온 마지막 sequence의 아이템만 구독한다.
- 설명이 어려운데 예제를보자!

    **예제코드**
    ```swift
    let one = PublishSubject<String>()
     let two = PublishSubject<String>()
     let three = PublishSubject<String>()

     let source = PublishSubject<Observable<String>>()

     let observable = source.switchLatest()
     let disposable = observable.subscribe(onNext: { print($0) })

     source.onNext(one)
     one.onNext("Some text from sequence one")
     two.onNext("Some text from sequence two")

     source.onNext(two)
     two.onNext("More text from sequence two")
     one.onNext("and also from sequence one")

     source.onNext(three)
     two.onNext("Why don't you see me?")
     one.onNext("I'm alone, help me")
     three.onNext("Hey it's three. I win")

     source.onNext(one)
     one.onNext("Nope. It's me, one!")

     disposable.dispose()

     /* Prints:
      Some text from sequence one
      More text from sequence two
      Hey it's three. I win
      Nope. It's me, one!
      */
    ```
    source observable로 들어온 마지막 sequence의 아이템만 구독하는 것을 볼 수 있다. 이 것이 switchLatest의 목적이다.

## sequence내의 요소들간 결합
### 1. reduce(::)
- Swift 표준 라이브러리의 reduce(:_:_) 처럼 내부의 요소들을 결합 시켜준다.
    **예제코드**
    ```swift
     let source = Observable.of(1, 3, 5, 7, 9)

     let observable = source.reduce(0, accumulator: +)
     observable.subscribe(onNext: { print($0) } )

     //위의 녀석을 풀어쓰자면 이러한 의미다!
     let observable2 = source.reduce(0, accumulator: { summary, newValue in
         return summary + newValue
     })
     observable2.subscribe(onNext: { print($0) })  
    /* Prints:
      1
      4
      9
      16
      25
     */
    ```
    reduce(:_:_)는 제공된 초기값(예제에서는 0)부터 시작해서 source observable이 값을 방출할 때마다 그 값을 가공한다.
    observable이 완료되었을 때, reduce(:_:_)는 결과값25을 방출하고 완료된다.

### 2. scan(_:accumulator:)
- reduce(:_:_) 처럼 작동하지만, 리턴값이 Observable이다.

    **예제코드**
    ```swift
    let source = Observable.of(1, 3, 5, 7, 9)

     let observable = source.scan(0, accumulator: +)
     observable.subscribe(onNext: { print($0) })
     /* Prints:
      1
      4
      9
      16
      25
     */
    ```
- scan(_:accumulator:)의 쓰임은 광범위 하다. 총합, 통계, 상태를 계산할 때 등 다양하게 쓸 수 있다.

