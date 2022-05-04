# Today I Learned.

## 학습내용
# RaceCondition, Thread Safe

## RaceCondition
>```swift
>import Foundation
>
>var cards = [1, 2, 3, 4, 5, 6, 7, 8, 9]
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        let card = cards.removeFirst()
>        print("말랑: \(card) 카드를 뽑았습니다!")
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        let card = cards.removeFirst()
>        print("밈: \(card) 카드를 뽑았습니다!")
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        let card = cards.removeFirst()
>        print("레드: \(card) 카드를 뽑았습니다!")
>    }
>}
>```
>cards 라는 카드 묶음에 1~9 까지의 카드가 있다.
>
>말랑, 밈, 레드 가 카드를 3장씩 나누어 가지려고 하는데 이때 순서에 상관없이 맨 앞번호의 카드를 가져가기로 했다.
>
>예상했던 실험 결과는 모두 async 로 비동기 방식으로 실행했기에 순서는 랜덤으로 가져갈것이라 예상했다.
>
>```swift
>말랑: 1 카드를 뽑았습니다!
>밈: 1 카드를 뽑았습니다!
>레드: 1 카드를 뽑았습니다!
>말랑: 2 카드를 뽑았습니다!
>밈: 5 카드를 뽑았습니다!
>말랑: 6 카드를 뽑았습니다!
>밈: 8 카드를 뽑았습니다!
>레드: 7 카드를 뽑았습니다!
>레드: 9 카드를 뽑았습니다!
>```
>실험결과는 예상하지 못한 문제가 생겼다.
>
>카드묶음에 1이라는 카드는 1개 밖에 없는데 1카드가 3장이된것이다.
>
>이유는 하나의 배열에 여러 스레드가 동시에 접근했기 때문에 발생한 문제다.
>
>하나의 값에 동시에 접근하는 경우 위와같은 상황이 발생할수있으며
>위와 같은 상황을 `Race Condition` 이라고 한다.

## Thread Safe
>`Race Condition` 이 발생하는 이유는 Swift 는 Thread Safe 하지 않기 때문이다.
>
>Thread Safe 하다는 것은 여러 스레드에서 동시에 접근이 불가능한것을 말한다.
>
>배열 뿐만 아니라 Swift 대부분의 타입들은 Thread Unsafe 하다.(여러 스레드에서 동시에 접근이 가능한 상태)

## DispatchSemaphore
>```swift
>class DispatchSemaphore : DispatchObject
>```
> DispatchSemaphore 는 공유자원에 접근할수 있는 스레드의 수를 제어해주는 역할을 한다.
> 
> 몇개의 스레드에 접근을 허용할것인지 제어할 수 있기 때문에 접근을 1개의 스레드만 허용한다면 Race Condition 을 방지할수 있게 된다.
> 
>DispatchSemaphore 는 semaphore count 를 카운트 하는 방식으로 동작한다.
>ARC 같은 느낌인가???
>
>예를 들어 하나의 스레드가 접근을 하면 count 에 -1 을 접근이 끝나면 count에 +1 을 해주어 설정해준 count 만큼만 스레드가 접근할수 있도록 관리해주는것이다.
>
>허용된 스레드의 수만큼 접근된 상태라면 다른 스레드는 접근하지 못하고 줄을 서서 기다리게 된다.
>
>count 가 다시 +1 이되면 기다리는 스레드 순서대로 접근이 허용된다.
>```swift
>let semaphore = DispatchSemaphore(value: 1) // >count = 1
>
>DispatchQueue.global().async {
>    semaphore.wait() // count -= 1
>
>    semaphore.signal() // count += 1
>}
>```
>wait() 은 값에 접근했다고 알리는 메서드
>signal() 은 일 다끝났다는 메서드

## RaceCondition 을 Thread Safe 하게 해결해보자
### semaphore
>```swift
>import Foundation
>
>var cards = [1, 2, 3, 4, 5, 6, 7, 8, 9]
>let semaphore = DispatchSemaphore(value: 1)
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        semaphore.wait()
>        let card = cards.removeFirst()
>        print("말랑: \(card) 카드를 뽑았습니다!")
>        semaphore.signal()
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        semaphore.wait()
>        let card = cards.removeFirst()
>        print("밈: \(card) 카드를 뽑았습니다!")
>        semaphore.signal()
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        semaphore.wait()
>        let card = cards.removeFirst()
>        print("레드: \(card) 카드를 뽑았습니다!")
>        semaphore.signal()
>    }
>}
>```
> semaphore 의 value 를 1로 설정하여 하나의 스레드만 접근이 가능하도록 초기화 해주었다.
> 
>각 스레드에서 cards 에 접근하기 전화 후에 semaphore 의 wait() 과 signal() 을 호출해주어 다른 스레드의 접근을 막아RaceCondition 문제를 해결할수있었다.

### serialQueue
>RaceCondition 이 발생한 이유는 여러 스레드에서 동시에 배열에 접근했기 때문이다.
>
>그러므로 serialQueue 를 활용해 문제를 해결할수도 있다.
>
>DispatchQueue.global().async 내부에서 실행될 작업들을 SerialQueue 로 작업을 시켜주면된다!
>```swift
>import Foundation
>
>var cards = [1, 2, 3, 4, 5, 6, 7, 8, 9]
>let pickCardsSerialQueue = DispatchQueue(label: >"PickCardsQueue")
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        pickCardsSerialQueue.sync {
>            let card = cards.removeFirst()
>            print("말랑: \(card) 카드를 뽑았습니다!")
>        }
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        pickCardsSerialQueue.sync {
>            let card = cards.removeFirst()
>            print("밈: \(card) 카드를 뽑았습니다!")
>        }
>    }
>}
>
>DispatchQueue.global().async {
>    for _ in 1...3 {
>        pickCardsSerialQueue.sync {
>            let card = cards.removeFirst()
>            print("레드: \(card) 카드를 뽑았습니다!")
>        }
>    }
>}
>
>```

