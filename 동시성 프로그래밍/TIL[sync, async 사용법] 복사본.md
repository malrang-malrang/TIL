# Today I Learned.

## 학습내용

# sync, async 사용법
>**동기(sync) 는 작업이 처리되기를 기다리는것.**
>**비동기(async)는 작업이 처리되기를 기다리지 않고 다른 스레드에서 처리되는것.**
>
>`sync`는 동기로 작업을 처리하겠다는 메서드.
>`DispatchQueue.global().sync {}` 를 호출한것은 `main` 스레드말고 새로운 스레드를 만들어서 작업을 처리할거야, 그런데 내작업이 끝날때까지 기다려야해 라는 의미가 된다.
>
>`async`는 비동기로 작업을 처리하겠다는 메서드.
>`DispatchQueue.global().async {}` 를 호출한것은
>`main`스레드 말고 새로운 스레드를 만들어서 작업을 처리할거야, 그런데 다음 작업이 있다면 끝날떄까지 기다리지말고 처리해도 좋아! 난 새스레드 에서 알아서 작업할게! 라는 의미가 된다.
>
>**위의 설명을 보고 생각한것**
>위의 설명을보고 `DispatchQueue.global().sync {}` 를 호출했을때 나는 새로운 스레드를 만들고 동기로 처리했기때문에 새로운 스레드 내의 작업들은 동기로 처리되고 메인스레드는 알아서 갈길 갈줄알았는데 예상과 달랐다.
>

## 예시코드
### main.async
```swift
print(2)
import Foundation

DispatchQueue.main.async {
    for _ in 1...5 {
        print("😀😀😀😀😀")
        sleep(1)
    }
}
DispatchQueue.main.async {
    for _ in 1...5 {
        print("🥶🥶🥶🥶🥶")
        sleep(2)
    }
}
print(15)
/* 출력
2
15
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
*/
```
>위의 예시코드를 보고 비동기로 처리했으니 섞여서 나올것이라 예상했다.
>하지만 다시생각해보니 메인스레드(Serial) 에서 진행되고있으니 순차적으로 진행하게된다.
>
>코드를 차례대로 슥훑고 지나가면서 큐에 저장하는 코드가 아닌 프린트 구문은 바로 출력되고, 큐에저장하는 코드(`DispatchQueue.main`)는 큐에 저장한뒤 저장된큐에서 하나씩 꺼내어 실행하게된다.

### global().async
```swift
import Foundation

DispatchQueue.global().async {
    for _ in 1...5 {
        print("😀😀😀😀😀")
        sleep(1)
    }
}

DispatchQueue.global().async {
    for _ in 1...5 {
        print("🥶🥶🥶🥶🥶")
        sleep(2)
    }
}

DispatchQueue.main.async {
    for _ in 1...5 {
        print("🥵🥵🥵🥵🥵")
        sleep(1)
    }
}

/* - 출력 (랜덤)
😀😀😀😀😀
🥶🥶🥶🥶🥶
🥵🥵🥵🥵🥵
😀😀😀😀😀
🥵🥵🥵🥵🥵
😀😀😀😀😀
🥶🥶🥶🥶🥶
🥵🥵🥵🥵🥵
😀😀😀😀😀
🥵🥵🥵🥵🥵
🥶🥶🥶🥶🥶
😀😀😀😀😀
🥵🥵🥵🥵🥵
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
*/
```
> 위의 예시코드는 추측한대로 메인스레드에 추가로 스레드를 두개더 생성해서 비동기 방식으로 실행했기때문에 출력되는 얼굴들은 어떤게 먼저 출력될지 알수없게된다.
> 
>각 스레드마다 작업이 처리되는 속도가 다를수 있다.
>그렇기 때문에 어떤것이 먼저 처리될지 정확한 순서를 알수 없다.

## global().sync
```swift
import Foundation

DispatchQueue.global().sync {
    for _ in 1...5 {
        print("😀😀😀😀😀")
        sleep(1)
    }
}

DispatchQueue.global().sync {
    for _ in 1...5 {
        print("🥶🥶🥶🥶🥶")
        sleep(2)
    }
}

for _ in 1...5 {
    print("🥵🥵🥵🥵🥵")
    sleep(1)
}
// main 스레드에서 동작하는 코드

/* - 출력
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
😀😀😀😀😀
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥶🥶🥶🥶🥶
🥵🥵🥵🥵🥵
🥵🥵🥵🥵🥵
🥵🥵🥵🥵🥵
🥵🥵🥵🥵🥵
🥵🥵🥵🥵🥵
*/
```
> 위의 예시코드는 추측대로 작업이끝날때까지 기다린후 다음 작업을 시작했다.

### main.sync
> 얘는 메인스레드에서 직접 호출`deadlock(교착상태)`에 빠지게 된다 직접호출 하면 안되는코드다.
> 메인스레드에서 직접호출하면 에러로 알려주게된다.
> 
>![](https://i.imgur.com/1KDJ4Qq.png)
>
>이유는 작업이 끝나기를 기다리는 `sync` 의 특성 때문이다.
>
>`sync`는 코드 블록이 끝나기 전까지 그 스레드는 멈춰있겠다는 뜻이다. 따라서 `main sync`를 호출하게되면 메인 스레드는 `sync` 코드 블록이 처리될때까지 기다려야 한다.
>
> 하지만 이때 메인스레드에서 실행되고 있었기 때문에 `sync` 의 코드 블록 역시 멈춰버리게된다.
>
>쉽게 요약하자면 `main,sync` 는 사용하면 메인스레드가 멈추게되니 에러가나고 `global.sync` 는 메인스레드를 멈추고 새로만든 스레드의 작업이 끝날때까지 기다려야한다.
>
>**`main,sync`를 사용할수있는 경우**
>메인 스레드에서 호출하지 않으면 된다.
>예를 들면 global스레드에서 main.sync 를 호출한경우
```swift
import Foundation
//새로운 스레드를 만들어서 비동기로 작업할거야!
DispatchQueue.global().async {
    //글로벌스레드는 메인의 sync 코드가 끝날때까지 멈출거야!
    DispatchQueue.main.sync {
        for _ in 1...5 {
            print("😀😀😀😀😀")
            sleep(1)
        }
    }
}

for _ in 1...5 {
    print("🥶🥶🥶🥶🥶")
    sleep(2)
}
```

## DispatchWorkItem
>`DispatchQueue`에서 코드 블록을 호출할때 >`DispatchWorkItem`을 활용해서 코드블록을 캡슐화 해줄수 있다.
>
>클로저랑 비슷하다.
>
>`DispatchWorkItem` 은 execute 파라미터를 통해 전달하여 사용한다.
>
>예시 코드를보자!
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

DispatchQueue.main.async(execute: yellow)
DispatchQueue.global().sync(excute: blue)
```

## asyncAfter
>`asyncAfter`는 `async` 메서드를 원하는 시간에 호출해줄 수 있는 메서드다.

```swift
DispatchQueue.global().asyncAfter(deadline: .now() + 5, execute: yellow)
```
위의 예시코드는 .now() 코드로부터 5초후에 yellow 라는 DispatchWorkItem 을 실행시킨다는 뜻이다.

## asyncAndWait
>`asyncAndWait`메서드를 사용해 비동기 작업이 끝나는 시점을 기다릴수 있다.
>비동기로 처리되는 어떤 동작이 끝나기를 의도적으로 기다려야 할때 사용할수 있다.

```swift
DispatchQueue.global().asyncAndWait(execute: yellow)
print("Finished!")
```
