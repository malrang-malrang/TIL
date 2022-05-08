# Today I Learned

## **학습내용**

### **cs 공부**
지금까지 맥을 사용하며 활성상태보기 안에 사용된 스레드, 프로세스, gpu등등 의 단어들 환경설정속에서 사용된 단어들
그것들에대해 한문장으로 정리할수없으나 무슨뉘앙스인지만 알고있던것들을 나는 알고있다고 생각하고 궁금해하지 않고 
의미하는 바에대해 찾아보지 않았던것 같다. 컴퓨터 전공은 아니지만 그래도 컴퓨터에 관심은 있었기에 대강의 용어들을 알고있다 라고 생각했는데 큰 오산이었다. 각잡고 딸딸 외우며 공부할것이아니라 틈틈히 찾아보고 머릿속으로 한문장씩 정리해볼수 있도록 하자.
M1pro 출시당시 실시간으로 시청했던적이 있었는데 그때 나오는 단어들을 모두 이해할수 없었기 때문에 어떤부분이 좋아지고 개선되었는지 알수 없었다. 용어들을 하나씩 찾아보고 머릿속으로 정리할수있게되면 다음에 볼땐 더 재밌게 볼수있을것 같다.

## **고민한점 **
```swift
func makeNonOverlappingNumber() -> Set<Int> {
var nonOverlappingNumber: Set<Int> = Set<Int>()
while nonOverlappingNumber.count < 3 {
nonOverlappingNumber.insert(randomInt(to: 1, from: 9))
```
이대로 사용할경우 운이 나빠 중복된값이 여러번 나올경우 반복문이 3번이아니라 여러번 돌아가게될수있어 비효율적이다.

## **해결방법**
```swift
var nonOverlappingNumber: [Int] = []
var range: Set<Int> = Set<Int>(to...from)
while nonOverlappingNumber.count < count {
guard let randomNumber: Int = range.randomElement() else { return nil }
nonOverlappingNumber.append(randomNumber)
range.remove(randomNumber)
```
이런식으로 Set타입의 변수에 값을 범위로 지정하여 중복되지 않은 여러개의 값을 갖게 한뒤
randomNumber에 Set타입의 변수가 가지고있는 값중 하나를 랜덤하게 할당해주고
randomNumber의 값을 최종적으로 다른곳에서 사용할 nonOverlappingNumber에 할당해준다.
그후 중복된값이 있으면 안되기에 nonOverlappingNumber에 사용된 값을 제거해준다.

문제는 해결했지만 프로젝트가 merge된후 알게된 shuffled() 메서드를 사용하면
좀더 쉽고 간결하게 문제를 해결할수있었을것 같다.
다음에 사용해봐야지..!

알게된점
해결방법에 작성된 shuffled()과 고차함수 reduce(),을 알게되었다.
shuffled() 는 순서를 섞은 속성들을 반환하는 메서드 이다.
```swift
let numbers = 0...9
let shuffledNumbers = numbers.shuffled()
// shuffledNumbers == [1, 7, 6, 2, 8, 9, 4, 3, 5, 0]
```
이런식으로 값의 범위를 지정한후 선언하게되면
 내부에는 중복되지않은 값들을 갖는 배열이 만들어지게된다.
 굉장히 유용하게 써먹을수 있을것같다.

reduce() 고차함수 (줄이다 라는뜻)
쉽게 말하면 피보나치 수열을 만들어주는 함수라고 생각하면될것같다.
배열의 Element들을  하나로 합치는 기능을 가진 고차함수!
```swift
let numbers: [Int] = [1,2,3,4,5]
var sum = numbers.reduce(0) { (result: Int, element: Int) -> Int in
    print("\(result) + \(element)")
    return result + element
}
0 + 1
1 + 2
3 + 3
6 + 4
10 + 5
```
이런식으로 피보나치 수열을 만들어 줄수 있다.
```swift
var sum = numbers.reduce(0) { (result, element)  in
    return result+element
}
var sum = numbers.reduce(0, +)
Var sum = numbers.reduce(0){$0 + $1}
```
이렇게 축약도 가능하다!

[Int]값이 아닌 [String] 에서 사용하게될경우
```swift
let charArray = ["a", "b", "c", "d"]
let charNewArray = charArray.reduce("result is ") {
    $0 + $1
}
print(charNewArray)
 //result is abcd
``` 
이런식으로 배열안에 있는 문자들을 더해줄수 있다!
joined() 처럼 사용할수있다!

## **참고자료**
[reduce()](https://developer.apple.com/documentation/swift/array/2298686-reduce)
[shuffled()](https://developer.apple.com/documentation/swift/array/2994757-shuffled)
