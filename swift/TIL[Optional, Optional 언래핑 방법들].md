# Today I Learned

## **학습내용**
Optional 언래핑 종류와 방법에대해 찾아보고 설명하는 시간을 가졌다.

## **옵셔널 이란**

값이 있을수도있고 없을수도 있다 를 나타낸다.

값이 없을수도 있는 경우에 옵셔널 (optionals) 을 사용합니다.

옵셔널은 2가지 가능성이 있습니다:

-   값이 있을경우 옵셔널을 풀어서 값에 접근하거나
-   값이 없을 수도 있습니다.

타입변환을 시도할때 실패할수도 있으므로 값이 없음을 반환하도록 옵셔널을 사용 해줄수있다.  
예시

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

convertedNumber는 타입변환이 실패할수 있으므로 값이 없음을 나타내는 Int? 타입이 됩니다.

## **nil 이란**

옵셔널 타입의 변수에 nil을 지정하여 값이 없음을 나타낼수 있습니다.

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404
serverResponseCode = nil
// serverResponseCode now contains no value
```

첫번째 예시 `var serverResponseCode: Int? = 404` 는  
serverResponseCode 에 현재 404,nil 두개중 하나의 값을 가지고있음을 나타냅니다.  
옵셔널 타입이기 때문에 어떤 값을 할당하더라도 값이 없을수도 있다고 표현 할수있습니다.  
두번째 예시 `serverResponseCode = nil` 는  
serverResponseCode 에 현재 값이없음을 나타냅니다.

```swift
NOTE
옵셔널이 아닌 상수와 변수에는 nil 을 사용할 수 없습니다. 
코드에서 상수 또는 변수가 값이 없는 상태에서 동작이 필요하다면 항상 해당 타입의 옵셔널 값으로 선언해야 합니다.
```

기본값이 없이 옵셔널변수를 정의하면 변수는 자동적으로 nil 로 설정됩니다.

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

surveyAnswer얘는 지금 nil 입니다.  
기본값을 넣어주지 않았기 때문에 값이 없습니다!

### **if 구문과 강제로 풀기 (If Statements and Forced Unwrapping)**

조건문을 사용하여  
변수의 값이 nil 일경우와 nil이 아닐경우를 나누어 분기를 만들어 줄수 있습니다.  
예시

```swift
if convertedNumber != nil {
print("convertedNumber contains some integer value.")
}
// Prints "convertedNumber contains some integer value."
```

이거 뜻은 convertedNumber 요놈이 nil 이 아니라면 if의 실행구문을 실행하라 라는 뜻이다.  
이때 convertedNumber은 옵셔널 타입이어야 한다.

## **옵셔널 언래핑 방법들**

Optional unwrapping  
옵셔널로 감싸져있는 값을 옵셔널이 아닌값으로 추출하는 작업  
(옵셔널 언래핑 대상이 nil 이면 안됩니다!!)

**1.강제 추출 Forced Unwrapping**  
옵셔널의 실제 값과 상관없이 강제로 값을 추출하는 것  
옵셔널이 nil일 때 강제추출을 사용하면 에러가 발생  
사용하게된다면 이옵셔널엔 무조건 값이 들어있을거야! 하고 확신할수 있는 상황에서만..사용하도록 하자

**2.옵셔널 바인딩 Optional Binding**  
옵셔널 값을 언래핑 하는 방법으로, 가장 많이 사용한다. (안전하다!!)  
옵셔널 바인딩 방법에는

-   `If let {}`
-   `guard let…else {}`  
    두가지가 있다

표현식  
`If let 임시상수이름 = 옵셔널값 { }`  
`guard let 임시상수이름 = 옵셔널값 else {}`  
타입 추론이되므로 타입어노테이션 생략 가능하다!

결과  
1.옵셔널값이 nil 이 아니라면 임시상수에 옵셔널 언래핑된 값이 저장되고 true를 반환한다  
2.옵셔널값이 nil 이라면 false 를 반환한다

그렇기 때문에 결과가 true 라면 이걸실행하고 false 라면 이걸실행해라 하고 분기를 나누어 사용할수있다!

If let 예시

```swift
var name: String? = “malrang”
If let malrang = name {
//name 이 nil 이 아닐때 (true일때)
print(malrang)
 } else {
//name 이 nil 일때 (false일때)
print(name)
}
```

주의할점

-   malrang은 if 구문 내부에서만 사용 할수있다.
-   name은 여전히 옵셔널 값임!

guard let 예시

```swift
var name: String? = “malrang”
guard let malrang  = name else { 
//name 이 nil 일경우
return 
}
//name 이 nil 이 아닐경우 guard 다음문장 실행 malrang 사용가능!
```

주의할점

-   함수(메서드)에서만 사용 가능
-   guard문 조건을 만족하지 못하면 else 로 처리되고 함수는 리턴 된다.
-   예외 처리해줄때 사용하면 좋다!  
    (클럽 문지기가 드레스코드 안맞추면 입장 불가 시키는 느낌?)  
    (님 드레스 코드 틀려요 입장 불가능합니다 = guard 조건 안맞아요 함수 내부기능 사용 불가합니다!)

**3 옵셔널 묵시적 추출 IUO - Implicitly Unwrapped Optional**  
강제 추출이나 옵셔널 바인딩처럼, 별도의 추출 과정을 거치지 않아도 자동으로 옵셔널이 해제되는 방법.

표현식  
타입 어노테이션 뒤에 ! 를 작성 해준다.  

예시  
`let malrang: String! = malrang`

-   IUO 얘도 옵셔널 타입이다.
-   IUO 도 내부적으로 강제추출을 하는것 같다.
-   옵셔널 값이 nil인 경우 IUO 를 사용해 추출하게되면 에러가 발생된다.

프로퍼티 지연 초기화를 하기위해 사용한다 는데.. 아직인 이해하지 못했다  
이해하게되면 업데이트 해두겠습니다.

**4.?? 연산자 - nil 병합 연산자 (Nil-Coalescing Operation)**  
coalescing을 한국어로 해석하면 "하나로 합치다" 라는 뜻임.  
옵셔널값이 nil 이면 값이있을수도 없을수도 있다 를 값이 없다 로 합쳐버리는 느낌..!?  
반대로 옵셔널에값이 있다면 얘는 값이 있다 로 합쳐버려서 언래핑 한다.  
옵셔널을 값이있는경우, 값이없는겨우 하나만 선택하게끔 한다 라고 표현하는 맞는것같다

표현식  
`옵셔널값 ?? 기본값`  
옵셔널 값이 nil이 아니면 옵셔널을 언래핑한 값을, nil이면 ?? 뒤의 기본값을 사용  
(옵셔널값과 기본값 무도 동일한 타입이어야 합니다!)

예시

```swift
let malrang: String? = nil
print(malrang ?? "mimm")
 // malrang에 값이 있으면 사용하고 없으면 기본값인 mimm 사용.

let eddy: String? = "eddy"
var donnie: String

donnie = eddy ?? "에!?"
print(donnie)
//donnie 에는 optional(eddy) 가 아닌 eddy가 들어간것을 확인할수 있다
//갑을 언래핑 하여 할당 해주었다는것을 알수 있다
```

**5.옵셔널 체이닝 Optional Chaining**

체이닝?? 연쇄라는 뜻  
옵셔널 체이닝은 옵셔널을 연쇄적으로 사용할때 사용하는것?

class, struct 같은 타입에서 .(dot)을통해 내부의 프로퍼티나 메서드에 연속적으로 접근할때 옵셔널값이 포함되어 있다면  
이것을 옵셔널 체이닝 이라고 한다!

예시

```swift
class Person {
var name: String?
var address: String?
}

Person.name?.address
```

예시처럼 프로퍼티가 옵셔널 이기때문에 값이 nil 일수 있기때문에 `?` 를 붙여 준다.  
만약 name 이 nil 이라면 뒤에있는 address를 검사하지 않고 표현식 전체가 nil 이된다.
