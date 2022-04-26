# Today I Learned.

## 학습내용

# GCD(Grand Central Dispatch)
>작업을 동시에 처리하기위해 스레드를 여러개 활용하는것은 불가피하다.
>그렇다면 어떻게 스레드를 관리해주어야 할까??
>
>일일이 개발자가 스레드를 관리하는것은 매우 번거롭고 힘든일이다.
>
>애플에서는 코드를 동기 혹은 비동기로 처리해준다면 시스템이 알아서 스레드를 관리해주는 방식을 제공하며 그것이 `GCD` 이다.
>
>`GCD`는 멀티 코어 환경과 멀티 스레드 환경에서 최적화된 프로그래밍을 할수 있도록 애플이 개발한 기술이다.
>
>`GCD` 를 사용하기 위해 `Dispatch` 라는 프레임워크를 사용해야하며 `Dispatch Queue` 라는 클래스를 사용해 대기열에서 작업의 순서를 동기, 비동기 처리만 해주면된다!

## Dispatch Queue
>`Dispatch Queue` 가 무엇이냐?
>`GCD`기술의 일부다.
>
>**단어를 직역해보자면**
>`Dispatch` = 보내다(파견하다),
>`Queue` = 대기열
>말그대로 대기열에 보낸다는 뜻이다.
>
>대기열에 **동기로 처리할것인지 비동기로 처리할것인지**, **단일스레드 인지 다중 스레드인지** 정해서 `Dispatch Queue(대기열)` 에 넣어주면 `GCD`가 알아서 스레드를 관리해주고 작업을 처리해준다.
>
>네이밍에 `Queue`가 들어간걸로 봐서 `FIFO(First in, First Out)`방식으로 대기열에 있는 작업들을 처리해주는걸 유추해볼수있다.

### Serial / Concurrent
>`DispatchQueue` 는 크게 `Serial Queue`와 `Concurrent Queue`로 두 가지로 구분한다.
>`Serial` 은 단일 스레드 에서만 작업을 처리하고
>`Concurrent`는 다중 스레드 에서 작업을 처리한다.
>
>`DispatchQueue`를 초기화할때 `attributes`를 `.concurrent`로 설정하지 않으면 기본값은 `Serial`이 된다.
>
>![](https://i.imgur.com/6IRyU5d.png)
>
### Main Thread
>![](https://i.imgur.com/z23VQtT.png)
>메인 스레드는 `Serial Queue`
>
>메인 스레드는 앱의 기본이 되는 스레드다.
>
>모든 앱은 적어도 하나이상의 스레드가 필요하다.
>
>모든 작업이 스레드에서 처리되기 때문이다!
>
>그렇기때문에 앱이 실행되는 동안 늘 메모리에 올라와있는 기본 스레드 이다. (앱의 생명주기와 같은 생명주기를 가진다)
>
>새로운 스레드를 생성하여 다중 스레드로 동작 하게될경우
>메인스레드는 여전히 메모리에 올라온 상태로 존재하며 
>메인스레드에서 필요한 만큼의 스레드가 파생되는것이다.
>
>**메인 스레드의 특징**
>1. 전역적으로 사용이 가능하다.
>2. global 스레드 들과 다르게 Run Loop가 자동으로 설정되고 실행된다. 이때 메인 스레드에서 동작하는 Run Loop 는 Main Run Loopf라고 한다.
>3. UI작업은 메인 스레드에서만 작업할수있다.

### main, global
>
>**사용방법**
>```swift
>// Serial Queue
>DispatchQueue.main
>// Concurrent Queue
>DispatchQueue.global()
>```
>위의 예시코드 처럼 `DispatchQueue`에 `Serial` 방식으로 보낼건지 `Concurrent` 방식으로 보낼건지 정해준다.
>
>`main`에 작업을 추가하면 `Serial`큐인 `main`스레드에서 작업을 처리하게된다.
>
>하나의 스레드에 쌓여서 하나씩 처리된다고 생각하면된다.
>
>그렇기때문에 `main` 스레드에만 작업을 쌓아두면 동시에 여러작업을 처리할수 없게된다.
>
>`global` 스레드는 `main`스레드가 아닌 작업을 처리하기 위해 새로 만들어진 스레드를 말한다.
>
>`global`방식은 `Concurrent` 방식이기 때문에 다른스레드와 동시에 작업이 가능해진다.
>
>작업을 global 스레드에 보내면 메모리에 올라왔다가 스레드에 저장된 모든 작업이 끝나면 메모리에서 자동으로 제거해준다.
>

### sync, async
>위에 설명한것처럼 DispatchQueue 로 GCD 를 구현하기 위해서는 동기로 처리할것인지 비동기로 처리할것인지도 정해주어야한다.
> 
>방법은 아래의 예시코드를 보자!
>```swift
>// 동기, sync
>DispatchQueue.main.sync {}
>DispatchQueue.global().sync {}
>
>// 비동기, async
>DispatchQueue.main.async {}
>DispatchQueue.global().async {}
>```
>
>음...간단히 요약해보자면 단일스레드(Serial) 로만 작업을하면 효율이 떨어질수 있으니 다중스레드(Concurrent)를 사용해 스레드 갯수를 늘려 여러개의 작업을 동시에 할수있게만들어줄수있따.
>
>그후 각각의 스레드에서도 작업들을 동기(sync) 즉 순서가 있게?작업을 하나씩 끝날때까지 기다리며 작업할것인지, 비동기(async)로 작업 할것인지 구분해주어야 한다.
