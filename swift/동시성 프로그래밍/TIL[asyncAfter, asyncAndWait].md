# Today I Learned.

## 학습내용
# asyncAfter, asyncAndWait
## asyncAfter
>asyncAfter 는 async 메서드를 원하는시간에 호출해줄 수 있는 메서드다.
>
```swift
DispatchQueue.global().asyncAfter(deadline: .now() + 5, execute: yellow)
```
>위의 예시코드는 .now() 로 부터 yellow 라는 DispatchWorkItem 을 실행시킨다는 뜻이다.

>deadline 은 스톱워치로 측정하듯 5초를 카운트하는 방식이다.
>wallDeadline 은 시스템(기기) 의 시간을 기준으로 카운트 하는것이다.

## asyncAndWait
>asyncAndWait 메서드는 비동기 작업이 끝나는시점을 기다릴수있다.
>
>비동기로 처리되는 어떤 동작이 끝나기를 의도적으로 기다려야할때 사용할수 있다.
```swift
DispatchQueue.global().asyncAndWait(execute: yellow)
print("Finished!")
```
>global().sync 랑 똑같은거 아닌가????
