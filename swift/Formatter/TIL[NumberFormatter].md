# Today I Learned.

## 학습내용
# Formatter, Number Formatter 
## Formatter
값을 다양한 텍스트 값으로 변환해 줄 수있는 추상 타입

Foundation 에서 지원하는 Formatter 는 다음과 같다.

1. ByteCountFormatter
2. DateFormatter
3. DateComponentsFormatter
4. DateIntervalFormatter
5. MeasurementFormatter
6. NumberFormatter
7. PersonNameComponentsFormatter

custom 하여 formatter 를 만들어 사용할수도 있다!

## Number Formatter 
Number Formatter 는 Formatter 를 상속 받는 추상 타입!
`class NumberFormatter : Formatter`

숫자들의 값이 큰 경우 콤마를 사용하여 구분해주는 경우가 생긴다.

이때 직접 로직을 만들어 사용할수도 있지만 
NumberFormatter 타입을 이용해 숫자형식을 변환할수 있다.

Int 혹은 Double 값을 넣어 String 값으로 반환 받을수 있는데 이때 반환된 값은 설정해둔 옵션에따라 3자리수 부터 콤마를 포함된 값으로 반환하거나, 숫자를 영어로 바꾸어 표기해주거나 옵션이 다양하다.

또 0이아닌 소수 0.1 등 소수부분을 표시하기 위한 옵션도 제공한다.
(소수의 유효자릿수 를 설정하는 옵션도 제공한다!)

옵션이 정말 많으니 공식 문서 사이트를 보고 참고하자!

[Number Formatter](https://developer.apple.com/documentation/foundation/numberformatter)

## Number Formatter 를 사용해보자

Number Formatter 를 인스턴스화 하여 생성해준다!
```swift
let numberFormatter = NumberFormatter()
```
사용할때는 생성해둔 numberFormmatter 에 옵션을 설정해준뒤
.string, .number 과 같은 메서드를 사용해
설정해둔 옵션을 거쳐 String? 값으로 반환해준다.

### **numberStyle**(숫자를 나타낼 스타일)
`numberStyle` 는 숫자를 어떤 스타일로 나타낼것인지 결정하는 프로퍼티

1. **.decimal**
```swift
let value: Double = 1234
let numberFormatter = NumberFormatter()
numberFormatter.numberStyle = .decimal
let result = numberFormatter.string(for: value)
// "1,234"
```
위의 예시에 사용된 .decimal 은 10 진법 형식으로 표현하도록 해준다.
(Decimal 타입으로 변환되는것 아님)
(로컬에 있는 위치의 10진법에 맞는 표기로 설정해주라는 뜻 이다.)
(나라 마다 .decimal 이 나타내는 표현이 달라진다고 한다.)

2. **.currency**
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234
numberFormatter.numberStyle = .currency
let result = numberFormatter.string(for: value)
// "$1,234.00"
```

3. **.percent**
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234
numberFormatter.numberStyle = .percent
let result = numberFormatter.string(for: value)
// "1,234"
```

### **roundingMode**(반올림)
1. **.ceilng, .floor**(정수부분 까지 올림, 내림)

**.ceilng**(정수 부분까지 올림)
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.3333
numberFormatter.roundingMode = .ceiling
let result = numberFormatter.string(for: value)
// 1235
```
**.floor**(정수 부분까지 내림)
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.3333
numberFormatter.roundingMode = .floor
let result = numberFormatter.string(for: value)
// 1234
```
2. **.down, .Up**(0을 향해 반올림, 0에서 반올림)

**.down**(0을 향해 반올림? Round towards zero.)
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.3333
numberFormatter.roundingMode = .down
let result = numberFormatter.string(for: value)
// 1234
```
**.up**(0에서 반올림? Round away from zero.)
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.3
numberFormatter.roundingMode = .up
let result = numberFormatter.string(for: value)
// 1235
```
3. **.halfUp, halfDown**(반올림, 반내림)

두가지의 차이점은 반으로 나누었을때 의 값을가지고 올려줄것인지 내려줄것인지 의 차이가 있다.
쉽게 설명하자면 10 을 중간인 5 를 halfUp을 하게되면 올림을 하게되고
halfDown 을 하게 되면 내림을 하게된다.

**.halfUp**
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.5
numberFormatter.roundingMode = .halfUp
let result = numberFormatter.string(for: value)
// 1235
```
**.halfDown**
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.5
numberFormatter.roundingMode = .halfDown
let result = numberFormatter.string(for: value)
// 1234
```
4. **.halfEven**(가장가까운쪽으로 반올림, 중앙일경우 짝수가 되도록 반올림)

**.halfEven**
```swift
let numberFormatter = NumberFormatter()
let value: Double = 1234.5
numberFormatter.roundingMode = .halfEven
let result = numberFormatter.string(for: value)
// 1234

let numberFormatter = NumberFormatter()
let value: Double = 1235.5
numberFormatter.roundingMode = .halfEven
let result = numberFormatter.string(for: value)
// 1236
```
위의 예시를 보면 알수 있듯이 반올림하는 기능이지만 등 거리 일경우 수가 짝수가 될수 있도록 반올림, 반내림 하게 된다.

## 정수 및 소수 자릿수 구성
1. minimumIntegerDigits
- 소수점 구분 기호 앞의 최소 자릿수
2. maximumIntegerDigits
- 소수점 구분 기호 앞의 최대 자릿수
3. maximumIntegerDigits
- 소수점 구분 기호 뒤의 최소 자릿수
4. maximumFractionDigits
- 소수점 구분 기호 뒤의 최대 자릿수

## 유효 자릿수 구성
1. usesSignificantDigits
- 최대 유효 자릿수를 사용하는지 나타내는 프로퍼티 (Bool값)
2. minimumSignificantDigits
- 최소 유효 자릿수
3. maximumSignificantDigits
- 최대 유효 자릿수

이외에도 여러가지 옵션들이 많으며 NumberFormatter 은 여러 나라에서 사용되는 앱에서 자주 사용되는것 같다 (국제 통화 를 비교해야 할때?)

다음에또 사용하게된다면 더 깊게 공부해보자..
