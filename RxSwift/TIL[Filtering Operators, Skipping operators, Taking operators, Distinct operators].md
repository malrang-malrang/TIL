# Today I Learned.

## 학습내용
# RxSwift Filtering Operators, Skipping operators, Taking operators, Distinct operators

## Filtering Operators
filtering operator를 사용해 `.next`이벤트를 통해 받아오는 값을 선택적으로 취하할 수 있다.(원하는놈만 걸러서 가져올수있다.)

기존 Swift의 filter(_:)와 비슷하다.

### 1. .ignoreElements()
https://rxmarbles.com/#ignoreElements
![](https://i.imgur.com/O5QZoVE.png)

ignoreElements는 `.next`이벤트를 무시한다.
`completed`, `error`같은 정지 이벤트는 허용한다.

**예시 코드**
```swift
     let strikes = PublishSubject<String>()
     let disposeBag = DisposeBag()
     
     strikes
         .ignoreElements()
         .subscribe({ _ in
             print("You're out!")
         })
         .disposed(by: disposeBag)
     
     strikes.onNext("X")
     strikes.onNext("X")
     strikes.onNext("X")
     
     strikes.onCompleted()
```
strikes subject를 구독하기전에 .ignoreElements() 를 사용하면 아무것도 프린트 되지 않는다.

.onNext()이벤트를 추가해도 아무일도 일어나지 않는다.

.onCompleted()하게되면 "You're out" 이 출력된다.

### 2. .elementAt
https://reactivex.io/documentation/operators/elementat.html
![](https://i.imgur.com/z2VKTp8.png)

Observable에서 방출된 n번째 요소만 처리하려는 경우가 있을 수 있다.
이 때 elementAt()을 쓸 수 있다.
이 것은 받고싶은 요소에 해당하는 index만을 방출하고 나머지는 무시한다.

**예제코드**
```swift
     let strikes = PublishSubject<String>()
     let disposeBag = DisposeBag()
     
     strikes
         .elementAt(2)
         .subscribe(onNext: { _ in
             print("You're out!")
         })
         .disposed(by: disposeBag)
     
     strikes.onNext("X")
     strikes.onNext("X")
     strikes.onNext("X")
```
strikes를 구독하기전에 .elementAt(2)를 추가한다.
2번째 인덱스 즉 3번째 값을 뱉는다는 뜻이다.
.completed나 .error가 아닌데도 콘솔에 "You're out!"이 출력된다.

### 3. .filter
![](https://i.imgur.com/y7x6Sc8.png)
https://reactivex.io/documentation/operators/filter.html

ignoreElements와 elementAt은 observable의 요소들을 필터링하여 방출한다.
filter는 필터링 요구사항이 한 가지 이상일 때 사용할 수 있다

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     Observable.of(1,2,3,4,5,6)
         .filter({ (int) -> Bool in
             int % 2 == 0
         })
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
of는 인자로받은 값을 next로 하나씩 전달해주는 오퍼레이터!(6개의 이벤트가 발생하겠죠?)
filter의 조건은 홀수를 제외하는 로직이다.(2로 나눴을때 나머지가 0인 경우에만 true)
filter의 조건에 부합하는 녀석을 출력하게된다.(2, 4, 6)이 출력된다.

## Skipping operators
### 1. .skip
![](https://i.imgur.com/7WutN3E.png)
https://reactivex.io/documentation/operators/skip.html

몇개의 요소를 skip하고 싶을수도 있다.
skip오퍼레이터는 첫번째 요소부터 n개의 요소를 skip하게 해준다.

**예제코드**
```swift
 let disposeBag = DisposeBag()
     
     Observable.of("A", "B", "C", "D", "E", "F")
         .skip(3)
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
여기도 마찬가지로 of를 썼으니 순서대로 next이벤트로 전달될텐데!
skip(3) 즉 처음부터 3번째의 next까지는 건너 뛰겠다는 뜻이다.
처음 세개 요소인 A, B, C 는 skip되고 뒤의 D, E, F가 출력된다.

### 2. skipWhile
![](https://i.imgur.com/2mxiH88.png)

https://reactivex.io/documentation/operators/skipwhile.html

구독하는 동안 모든 요소를 필터링하는 filter와는 달리, .skipWhile은 어떤 요소를 skip하지 않을 때까지 skip하고 종료하는 연산자이다.

skipwWhile은 skip할 로직을 구성하고 해당 로직이 false 되었을 때 방출한다. filter와 반대다.

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     Observable.of(2, 2, 3, 4, 4)
         .skipWhile({ (int) -> Bool in
             int % 2 == 0
         })
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
of는 이제생략..!
skipWhile을 사용했는데 조건은 filter와 반대로 false값만 방출되겠죠??
2로 나눴을때 나머지가 0이 아닌 값들만 방출하게되니까!
3을 출력하게된다.

### 3. skipUntil
![](https://i.imgur.com/k4IWU13.png)

https://reactivex.io/documentation/operators/skipuntil.html

지금까지의 필터링은 고정 조건에서 이루어졌다. 만약에 다른 observable에 기반한 요소들을 다이나믹하게 필터하고 싶을때 사용할수있다.

skipUntil은 다른 observable이 시동할 때까지 현재 observable에서 방출하는 이벤트를 skip 한다.

skipUntil은 다른 observable이 .next이벤트를 방출하기 전까지는 기존 observable에서 방출하는 이벤트들을 무시하는 것이다.

무슨소린지 잘 이해가 안가니까 코드를보자!
**예제코드**
```swift
 let disposeBag = DisposeBag()
     
     let subject = PublishSubject<String>()
     let trigger = PublishSubject<String>()
     
     subject
         .skipUntil(trigger)
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     subject.onNext("A")
     subject.onNext("B")
     
     trigger.onNext("X")
     
     subject.onNext("C")
```
subject를 구독(subscribe)하기 전에 skipUntil을 사용해 trigger 를 추가했다.
subject에 A, B 를 전달했다.
하지만 콘솔창에는 아무것도 출력되지 않는다.
trigger에 X를 전달했다.
그후 subject에 C를 전달했는데 이제서야 콘솔창에 C가 출력된다.
이전까지는 skipUntil 이 막고있기 때문이다.
skipUntil의 인자에 들어간 Observable이 .next를 방출 한후에서야 이벤트를 전달할수있게되는것이다.(이전까지는 이벤트 무시됨)

## Taking operators
### 1. take
![](https://i.imgur.com/TcQB3nW.png)

https://reactivex.io/documentation/operators/take.html

Taking은 skipping의 반대 개념이다.
어떤 요소를 취하고 싶을때 사용하는 오퍼레이터다.

그림을 보면 take()를 통해, 처음 2개의 값을 취한 것을 알 수 있다.

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     Observable.of(1,2,3,4,5,6)
         .take(3)
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
skip 반대니까 처음부터 세번째 요소 까지의 값을 취하면된다!
그러면 1, 2, 3이 출력된다!

### 2. takeWhile
![](https://i.imgur.com/V2SNcPS.png)

https://reactivex.io/documentation/operators/takewhile.html

takeWhile은 skipWhile의 반대로 작동한다.

takeWhile 구문 내에 설정한 로직에서 true에 해당하는 값을 방출하게 된다.

filter쓰면되는거 아닌가? 궁금한데 나중에 사용해보고 추가로 작성해보자!

### 3. enumerated
방출된 요소의 index를 참고하고 싶은 경우가 있을 것이다. 이럴 때는 enumerated 연산자를 확인할 수 있다.

기존 Swift의 enumerated 메소드와 유사하게, Observable에서 나오는 각 요소의 index와 값을 포함하는 튜플을 생성하게 된다.

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     Observable.of(2,2,4,4,6,6)
         .enumerated()
         .takeWhile({ index, value in
             value % 2 == 0 && index < 3
         })
         .map { $0.element }
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
.enumerated를 이용하여 index와 값을 가지는 튜플을 받아낸다.
.takeWhile을 이용하여, 튜플 각각의 요소들을 확인한다.
takeWhile의 조건이 2로나눴을때 나머지가 0인경우, index가 3번째 보다 작을경우 모두 충족할때만 취하게 된다.
.map을 사용해 추출한 튜플중 값(value)만 취하게한다.
요렇게 하면 2, 2, 4가 출력된다.

### 4. takeUntil
![](https://i.imgur.com/YBAJqsC.png)

https://reactivex.io/documentation/operators/takeuntil.html

skipUntil처럼 takeUntil도 있다.
trigger가 되는 Observable이 구독되기 전까지의 이벤트값만 받는 것이다.

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     let subject = PublishSubject<String>()
     let trigger = PublishSubject<String>()
     
     subject
         .takeUntil(trigger)
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     subject.onNext("1")
     subject.onNext("2")
     
     trigger.onNext("X")
     
     subject.onNext("3")
```
takeUntil() 메소드를 통해 trigger를 연결해주고 구독한다.
subject에 1, 2를 onNext를 통해 추가한다. (각각 프린트 된다)
trigger에 onNext 이벤트를 추가한다. 이걸 통해서 subject에서 값을 취하는 게 멈출 것이다.
subject에 새로운 값3을 onNext 이벤트로 추가한다. (3은 출력안됨)

RxCocoa 라이브러리의 API를 사용하면 dispose bag에 dispose를 추가하는 방식 대신 takeUntil을 통해 구독을 dispose 할 수 있다. 아래의 코드를 살펴보자.

```swift
someObservable
     .takeUntil(self.rx.deallocated)
     .subscribe(onNext: {
         print($0)
     })
```
이전의 코드에서는 takeUntil의 trigger로 인해서 subject의 값을 취하는 것을 멈췄었다.

여기서는 그 trigger 역할을 self의 할당해제가 맡게 된다. 보통 self는 아마 뷰컨트롤러나 뷰모델이 될것이다.

## Distinct operators(고유 오퍼레이터) 
중복해서 이어지는 값을 막아주는 오퍼레이터다.

### 1. distinct
![](https://i.imgur.com/BMctGl7.png)

https://reactivex.io/documentation/operators/distinct.html

Observable에서 방출하는 중복 항목 억제

위의 사진의 첫번째 스트림 마블을 보면 1, 2, 2, 1, 3인데
distinct를 사용하고 변경된 스트림을 보니 
1, 2, 3만 방출됐다!
중복된 값은 제거된것이다!

### 2. distinctUntilChanged
![](https://i.imgur.com/wLzHsR3.png)

distinctUntilChanged는 연달아 같은 값이 이어질 때 중복된 값을 막아주는 역할을 한다.
2는 연달아 두 번 반복되었으므로 뒤에 나온 2가 배출되지 않았다.
1은 중복이긴 하지만 연달아 반복된 것이 아니므로 그대로 배출된다.

**예제코드**
```swift
let disposeBag = DisposeBag()
     
     Observable.of("A", "A", "B", "B", "A")
         .distinctUntilChanged()
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
```
위의 설명을 이해했다면 쉽다! 
A, B, A 가 출력된다!

