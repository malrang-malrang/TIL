# Today I Learned.

## 학습내용
# RxSwift Transforming Operators

## Transforming Operators
- RxSwift의 연산자 카테고리에서 제일 중요한 연산자라 할 수 있는 변환연산자(transforming Operators)
- 변환연산자는 subscriber를 통해 observable에서 데이터를 준비하는 것 같은 모든 상황에서 쓰일 수 있다.
- 앞서 본 filter처럼 여기서도 map(_:)이나 flatMap(_:)같이 Swift 표준 라이브러리와 RxSwift 간에 유사점이 있는 연산자들을 확인할 수 있다.

### 1. toArray()
![](https://i.imgur.com/ASS9Ph8.png)

- Observable의 독립적 요소들을 array로 넣는 가장 편리한 방법은 toArray를 사용하는 것이다.

    위의 marble diagram 을 보면 toArray는 observable sequence의 요소들은 array의 요소들로 넣는다.
    그리고 이렇게 구성된 array를 .next 이벤트를 통해 subscriber에게 방출한다.
    
    **예시코드**
    ```swift
    let disposeBag = DisposeBag()
     Observable.of("A", "B", "C")
         .toArray()
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
    ```
    요녀석은 array로 바뀐다고했다.
    출력은 요렇게! ["A", "B", "C"]
    
### 2. map()

- RxSwift의 map 연산자는 Observable 에서 동작한다는 점만 제외하면 Swift 표준 라이브러리의 map과 같다.

    map은 각각의 요소에서 10을 곱하는 클로저를 갖는다.
    
    **예시코드**
    ```swift
    let disposeBag = DisposeBag()
     
     let formatter = NumberFormatter()
     formatter.numberStyle = .spellOut
     
     Observable<Int>.of(1, 2, 3)
         .map {
             string($0) ?? ""
         }
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
    ```
    요녀석 출력은 1, 2, 3 차례대로 출력된다.
    map 사용하던것처럼 사용하면 되겠다.
    
### 3. enumerated

- 이녀석 앞에서 본녀석이다.
- 방출된 요소의 index를 사용하고싶은경우 사용하던 녀석.
- 기존 Swift의 enumerated 메소드와 유사하게, Observable에서 나오는 각 요소의 index와 값을 포함하는 튜플을 생성하게 된다.

    **예시코드**
    ```swift
     let disposeBag = DisposeBag()
     
     Observable.of(1, 2, 3, 4, 5, 6)
         .enumerated()
         .map { index, interger in
             index > 2 ? interger * 2 : interger
         }
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
    ```
    
    enumerated를 사용해서 요소들의 값과 index를 갖는 tuple을 만든다.
map을 사용해서 tuple의 값을 살펴본다. 만약 요소들의 index가 2보다 크면, 값에 2를 곱한 것을 리턴한다. 그렇지 않으면, 해당 값을 그대로 리턴한다.
해당 Observable을 구독하고 값을 방출한다.

## 내부의 Observable 변환하기
- '만약 Observable' 속성을 갖는 Observable은 어떻게 사용할 수 있을까?

    **예제코드를 위한 타입**
    ```swift
     struct Student {
     var score: BehaviorSubject<Int>
     }
    ```
    Student는 BehaviorSubject<Int> 속성의 score라는 속성을 갖는 struct다.
    RxSwift는 flatMap 연산자 내에 몇 가지 연산자를 가지고 있다. 이들은 observable 내부로 들어가서 observable 속성들과 작업한다.
    
### 1. flatMap
![](https://i.imgur.com/ve5HOHn.png)
    
- Observable sequence의 각 요소를 Observable sequence에 투영하고 Observable sequence를 Observable sequence로 병합한다.
    
    위의 이미지를 보면 동그란 마블을 마름모 마블로 변형시켜주는것 같은데 흠...
    
    **예제코드**
    ```swift
    let disposeBag = DisposeBag()
     
     let ryan = Student(score: BehaviorSubject(value: 80))
     let charlotte = Student(score: BehaviorSubject(value: 90))
     
     let student = PublishSubject<Student>()
     
     student
         .flatMap{
             $0.score
         }
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     student.onNext(ryan)    // Printed: 80
     
     ryan.score.onNext(85)   // Printed: 80 85
     
     student.onNext(charlotte)   // Printed: 80 85 90
     
     ryan.score.onNext(95)   // Printed: 80 85 90 95
     
     charlotte.score.onNext(100) // Printed: 80 85 90 95 100
    ```

    Student 타입의 source subject를 만든다.
    flatMap을 사용해서 student subject와 subject가 갖는 score값에 접근한다. score를 수정하지 말고 일단 통과하게 하자.
.next 이벤트의 요소를 구독하여 프린트되게 한다. 하지만 이 때까지는 아무 것도 출력되지 않는다.
    ryan이라는 Student 인스턴스를 .onNext 이벤트를 통해 추가한다. 이렇게 하면 ryan의 score값이 출력된다.
    ryan의 score값을 변경해보자. 이렇게 하면 ryan의 새 점수가 출력된다.
또 다른 Student 인스턴스인 chalotte를 추가하자. 이렇게 하면 chalotte의 점수가 출력된다.
    ryan의 점수를 다시 바꿔보자. 역시 변경된 값이 출력된다.

    요약하자면, flatMap은 각 Observable의 변화를 계속 지켜본다.
    
    위의 마블그림을 설명하자면 첫번째 빨간 등록된 빨간마블 요녀석은 옵저버블인데 onNext로 옵저버블을 전달하게되면 전달된 옵저버블을 게속 지켜보게된다. 구독하게되는것이다.
    그러므로 빨간마블의 값이 변경되면 구독하고 있었기때문에 변경된 값이 추가로 전달되게 된다.
    
    flatMap()쓰고 next로 옵저버블을 넣게되면 전달된 옵저버블들을 모두 구독하며 옵저버블들의 값이 바뀌게되면 게속 추가되어 전달된다.
    
### 2. flatMapLatest(0)
- observable sequence의 각 요소들을 observable sequence들의 새로운 순서로 투영한 다음, observable sequence들의 observable sequence 중 가장 최근의 observable sequence 에서만 값을 생성한다.
- 위의 flatMap()에서 가장 최신의 값만 확인하고싶을때 요녀석을 사용한다.
- flatMapLatest = map + switchLatest
    - map과 switchLatest 연산자를 합친 것이 flatMapLatest라고 할 수 있다.
    - switchLatest는 가장 최근의 observable 에서 값을 생성하고 이전 observable을 구독 해제한다.

    **예제코드**
    ```swift
    let disposeBag = DisposeBag()
     
     let ryan = Student(score: BehaviorSubject(value: 80))
     let charlotte = Student(score: BehaviorSubject(value: 90))
     
     let student = PublishSubject<Student>()
     
     student
         .flatMapLatest {
             $0.score
     }
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     student.onNext(ryan)
     ryan.score.onNext(85)
     
     student.onNext(charlotte)
     
     ryan.score.onNext(95)
     charlotte.score.onNext(100)
    ```
    flatMap에서의 코드와 다른 점은 단 하나, 변경된 ryan의 점수인 95가 여기서는 반영되지 않는다는 점이다.
왜냐하면, flatMapLatest는 이미 charlotte의 최근 observable로 전환 했기 때문이다.
    
    요약하면 flatMap()은 여러개의 옵저버블을 게속 구독하지만 
    flatMapLatest()는 가장 마지막에 전달받은 옵저버블만을 구독하게된다.
    이전에 구독하던놈 값 바껴도 전달안해준다!
    
    #### 언제 사용할까?
    - flatMapLatest는 네트워킹 조작에서 가장 흔하게 쓰일 수 있다.
    - 사전으로 단어를 찾는 것을 생각해보자. 사용자가 각 문자 s, w, i, f, t를 입력하면 새 검색을 실행하고, 이전 검색 결과 (s, sw, swi, swif로 검색한 값)는 무시해야할 때 사용할 수 있을 것이다.
    
## observable의 이벤트 관찰하기(에러 처리할때 사용함)
- observable을 observable의 이벤트로 변환해야할 수 있다.
- 보통 observable 속성을 가진 observable 항목을 제어할 수 없고, 외부적으로 observable이 종료되는 것을 방지하기 위해 error 이벤트를 처리하고 싶을 때 사용할 수 있다.
    
    **예제코드**
    ```swift
    enum MyError: Error {
         case anError
     }
     
     let disposeBag = DisposeBag()
     
     let ryan = Student(score: BehaviorSubject(value: 80))
     let charlotte = Student(score: BehaviorSubject(value: 100))
     
     let student = BehaviorSubject(value: ryan)
     
     let studentScore = student
         .flatMapLatest{
             $0.score
     }
     
     studentScore
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     ryan.score.onNext(85)
     ryan.score.onError(MyError.anError)
     ryan.score.onNext(90)
     
     student.onNext(charlotte)
    ```
    
