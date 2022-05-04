# Today I Learned.

## 학습내용
# CompletionHandler, async

## CompletionHandler
>구현되어있는 코드들 중 CompletionHandler, Completion 클로저는 함수의 실행 순서를 보장받을수 있는 클로저다.
>
>escaping 클로저는 함수의 실행이 끝나면 함수의 밖에서 실행되는 작업들이다.
>
>completionHandler와 같은 클로져를 사용하는 경우에는 비동기 메서드일 때에도 작업이 종료되는 시점을 추적할 수 있고, 순서를 보장받을 수 있다.
>
>대표적인 예시로 URLSession 이 있다.

## async
> async 는 3가지의 파라미터가 존재한다.
>```swift
>func async(group: DispatchGroup? = nil, qos: DispatchQoS = .unspecified, flags: DispatchWorkItemFlags = [], execute work: @escaping () -> Void)
>```

### group
>`DispatchQueue` 의 `async` 코드 블록을 묶어서 관리해주는`DispatchGroup`이다.
>```swift
>class DispatchGroup : DispatchObject
>```
>`DispatchGroup` 은 비동기적으로 처리되는 작업들을 그룹으로 묶어, 그룹 단위로 작업 상태를 추적할수 있는 기능이다. 
>
>async 들을 묶어 그룹의 작업이 끝나는 시점을 추적하여 어떠한 동작을 수행시킬수 있다.
>
>이때 묶어줄 async 작업들이 같은 큐, 같은 스레드에 있지 않아도 된다.
>
>DispatchGroup 은 async 에서만 사용할수 있다.

#### group에 등록하기
>DispatchGroup 은 특별한 초기화 구문이 없다.
>init() 메서드로 초기화해 필요한 만큼 인스턴스를 만들어서 사용하면된다.
>함께 관리할 작업들에는 같은 인스턴스를 지정해 주어야 한다.
>
>DispatchGroup 을 사용하는 방법은 2가지가 있다.
>1. async 를 호출하면서 파라미터로 group 을 지정해준다.
>2. enter, leave 를 코드의 앞뒤로 호출하여 group 을 지정해준다.
>
>enter와 leave 는 DispatchGroup 이 enter() 부터 leave() 까지 포함된다 라는 의미이다.
>
```swift
let group = DispatchGroup()

// enter, leave를 사용하지 않는 경우
DispatchQueue.main.async(group: group) {}
DispatchQueue.global().async(group: group) {}

// enter, leave를 사용하는 경우
group.enter()
DispatchQueue.main.async {}
DispatchQueue.global().async {}
group.leave()
```
> 이렇게 원하는 작업들을 group 으로 묶어준뒤 묶어준 그룹에 notify(), wait() 으로 작업을 추적해줄 수 있다.

#### notify(group 의 작업이끝난후 원하는 동작 실행)
>notify는 DispatchGroup의 업무 처리가 끝나는 시점에 원하는 동작을 수행하기 위한 메서드다.
> 어렵지 않다.
> 예시를 보자!
```swift
import Foundation

let red = DispatchWorkItem {
    for _ in 1...5 {
        print("🥵🥵🥵🥵🥵")
        sleep(1)
    }
}

let yellow = DispatchWorkItem {
    for _ in 1...5 {
        print("😀😀😀😀😀")
        sleep(1)
    }
}

let blue = DispatchWorkItem {
    for _ in 1...5 {
        print("🥶🥶🥶🥶🥶")
        sleep(2)
    }
}

let group = DispatchGroup()

DispatchQueue.global().async(group: group, execute: blue)
DispatchQueue.global().async(group: group, execute: red)

// group.enter()
// DispatchQueue.global().async(execute: blue)
// DispatchQueue.global().async(execute: red)
// group.leave()

group.notify(queue: .main) {
    print("모든 작업이 끝났습니다.")
}
```
> 예시 코드를 보면 group 의 모든 작업이 끝나면 notify 의 코드블럭이 실행된다.
> 
> 이때 notify 의 파라미터인 queue 는 코드블럭을 실행시킬 queue 를 말한다.

#### wait
>wait 은 DispatchGroup의 수행이 끝나기를 기다리는 메서드다.
>notify 와 달리 별도의 코드블록을 실행하지 않는다.
```swift
let group = DispatchGroup()

DispatchQueue.global().async(group: group, execute: blue)
DispatchQueue.global().async(group: group, execute: red)

group.wait()
print("모든 작업이 끝났습니다.")

// group.wait(timeout: 10)
// print("모든 작업이 끝났습니다.")
```
>wait 메서드는 timeout 파라미터를 설정 해줄수있다.
>
>timeout에 10을 전달하면 group 의작업을 시작한후 10초 동안만 기다리는것이다.
>
>10초가 지나도 group의 작업이 끝나지 않았다면 더이상 기다리지 않고 다음 코드를 실행한다.

### Qos
>자원을 어떻게 분배할것인지의 우선도 6개가 있다.
>**1. User-interactive**
>**2. User-initiated**
>**3. Default**
>**4. Utility**
>**5. Background**
>**6. Unspecified**

### flags
>DispatchWorkitemFlags 타입의 값을 받는 파라미터다.
>코드 블럭을 실행할때 적용될 추가 속성을 결정한다.
>
>기본 값으로는 아무 속성도 부여하지 않는다.
>flags 값은 OptionSet 이므로 여러가지의 속성을 한 번에 부여할수도 있다.
>
>**1. assingCurrentContext**
>>코드 블록을 실행하는 context(Queue혹은 스레드)의 속성을 상속받는다. 
>>
>>Qos와 같은 속성을 으로 한다는 얘기임.
>
>**2. barrier**
>>concurrent queue 환경에서 barrier(장벽, 차단) 역할을 한다.
>>
>>barrier 속성의 코드 블록이 실행되기 전에 실행되었던 코드들은 완료까지 실행되고, barrier 속성의 코드 블록이 실행되기 전까지 다른 코드 블록은 실행되지 않는다.
>
>**3. detached**
>>실행할 코드 블록에 실행중인 context(Queue 혹은 스레드)의 속성을 적용하지 않는다.
>
>**4. enforceQos**
>>실행중인 context 의 Qos보다 실행할 코드 블록의 Qos에 더 높은 우선순위를 부여한다.
>
>**5. inheritQos**
>>enforceQos 와 반대로 실행중인 context의 Qos 에 더높은 우선 순위를 부여한다.
>
>**6. noQos**
>>Qos 를 할당하지 않고 코드 블록을 실행시킨다.
>>assingCurrentContext 보다 우선되는 속성이다.
