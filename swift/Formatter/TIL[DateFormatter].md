# DateFormatter
![](https://i.imgur.com/MxCvTjN.png)

날짜와 텍스트 표현 사이를 변환하는 포맷터입니다.
Date를 text로 바꿔주는놈!

## Declaration
```swift
class DateFormatter : Formatter
```
NumberFormatter 처럼 Formatter 상속받는 친구니까 NumberFormatter 랑 비슷하겟죠..?

## Overview
DateFormatter 인스턴스는 NSDate 개체의 문자열 표현을 만들고 날짜 및 시간의 텍스트 표현을 NSDate 개체로 변환합니다. 사용자가 볼 수 있는 날짜 및 시간 표현을 위해 DateFormatter는 다양한 지역화된 사전 설정 및 구성 옵션을 제공합니다. 날짜 및 시간에 대한 고정 형식 표시의 경우 사용자 정의 형식 문자열을 지정할 수 있습니다.

ISO 8601 형식의 날짜 표시로 작업할 때는 ISO 8601 DateFormatter를 대신 사용하십시오.

두 NSDate 개체 간의 간격을 나타내려면 DateIntervalFormatter를 대신 사용합니다.

NSDateComponents 개체에서 지정한 시간을 나타내려면 DateComponentFormatter를 대신 사용하십시오.

## Working With User-Visible Representations of Dates and Times
사용자에게 날짜를 표시할 때 특정 필요에 따라 날짜 형식자의 dateStyle 및 timeStyle 속성을 설정합니다.
예를들어 시간을 표시하지 않고 월, 일 및 연도를 표시하려면 dateStyle 속성을 .long으로 설정하고 timeStyle 속성을 .none으로 설정합니다. 
반대로, 시간만 표시하려면 dateStyle 속성을 .none으로 설정하고 timeStyle 속성을 .short로 설정합니다. 
dateStyle 및 timeStyle 속성의 값을 기반으로 DateFormatter는 지정된 로케일에 적합한 지정된 날짜 표현을 제공합니다.

**예제코드**
```swift
let dateFormatter = DateFormatter()
dateFormatter.dateStyle = .medium
dateFormatter.timeStyle = .none
 
let date = Date(timeIntervalSinceReferenceDate: 118800)
 
// US English Locale (en_US)
dateFormatter.locale = Locale(identifier: "en_US")
print(dateFormatter.string(from: date)) // Jan 2, 2001
 
// French Locale (fr_FR)
dateFormatter.locale = Locale(identifier: "fr_FR")
print(dateFormatter.string(from: date)) // 2 janv. 2001
 
// Japanese Locale (ja_JP)
dateFormatter.locale = Locale(identifier: "ja_JP")
print(dateFormatter.string(from: date)) // 2001/01/02
```
위의 설명 읽었다면 충분히 이해할수있다!

medium 설정은 위의 설명에없었지만 대강 date(연, 월, 일)를 중간싸이즈? 로 설정하는것같다.
long은 모든 정보를 표현하는것이라면 medium은 조금 간소화된 표현? 정도로 이해하면된다!

미리 정의된 스타일을 사용하여 만들 수 없는 형식을 정의해야 하는 경우 setLocalizedDateFormatFromTemplate(_:)를 사용하여 템플릿에서 지역화된 날짜 형식을 지정할 수 있습니다.

무슨소리냐면 .long, .midium 등등 미리 정의된 스타일이아니라 새로운 스타일로 커스텀해야할경우 setLocalizedDateFormatFromTemplate(_:) 메서드 사용해서 만들수 있다는 소리다!

```swift
let dateFormatter = DateFormatter()
let date = Date(timeIntervalSinceReferenceDate: 410220000)
 
// US English Locale (en_US)
dateFormatter.locale = Locale(identifier: "en_US")
// set template after setting locale
dateFormatter.setLocalizedDateFormatFromTemplate("MMMMd")
// December 31
print(dateFormatter.string(from: date))
 
// British English Locale (en_GB)
dateFormatter.locale = Locale(identifier: "en_GB")
// set template after setting locale
print(dateFormatter.string(from: da
dateFormatter.setLocalizedDateFormatFromTemplate("MMMMd")
// 31 December       
print(dateFormatter.string(from: date))
```

## Working With Fixed Format Date Representations

```
import
macOS 10.12 이상 또는 iOS 10 이상에서는 ISO 8601 날짜 표현으로 작업할 때
ISO 8601 DateFormatter 클래스를 사용합니다.
```

RFC 3339와 같은 고정 형식 날짜로 작업할 때 형식 문자열을 지정하도록 dateFormat 속성을 설정합니다.
대부분의 고정 형식의 경우 로케일 속성을 POSIX 로케일("en_US_POSIX")로 설정하고 timeZone 속성을 UTC로 설정해야 합니다.

```swift
let RFC3339DateFormatter = DateFormatter()
RFC3339DateFormatter.locale = Locale(identifier: "en_US_POSIX")
RFC3339DateFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ssZZZZZ"
RFC3339DateFormatter.timeZone = TimeZone(secondsFromGMT: 0)
 
/* 39 minutes and 57 seconds after the 16th hour of December 19th, 1996 with an offset of -08:00 from UTC (Pacific Standard Time) */
let string = "1996-12-19T16:39:57-08:00"
let date = RFC3339DateFormatter.date(from: string)
```
요놈은 정확히 뭔지 모르겠는데 인터넷 날짜? 관련된놈인것 같다.

## Thread Safety
iOS 7 이상에서는 NSDate Formatter가 스레드 세이프입니다.

64비트 앱에서 최신 동작을 사용하는 한 macOS 10.9 이상에서 NSDateFormatter는 스레드 세이프입니다.

이전 버전의 운영 체제 또는 기존 포맷터 동작을 사용하거나 macOS에서 32비트로 실행할 경우 NSDateFormatter는 스레드 세이프가 아니므로 여러 스레드에서 동시에 날짜 포맷터를 변형해서는 안 됩니다.

## Topics
요녀석 메서드, 프로퍼티가 많다.
자주 사용될것같은놈들만 정리해두자!
다른놈들이 필요하면 공식문서를 참고하자!
[DateFormatter](https://developer.apple.com/documentation/foundation/dateformatter)

### 개체 변환
- func date(from: String) -> Date?
   - 시스템이 수신자의 현재 설정을 사용하여 해석하는 지정된 문자열의 날짜 표현을 반환합니다.
- func string(from: Date) -> String
   - 시스템이 수신기의 현재 설정을 사용하여 형식을 지정하는 지정된 날짜의 문자열 표현을 반환합니다.
- class func localizedString(from: Date, dateStyle: DateFormatter.Style, timeStyle: DateFormatter.Style) -> String
   - 지정된 날짜 및 시간 스타일을 사용하여 시스템이 현재 로케일에 대해 형식을 지정하는 지정된 날짜의 문자열 표현을 반환합니다.

### 형식 및 스타일 관리
- var dateStyle: DateFormatter.Style
   - 수신기의 날짜 스타일입니다.
- var timeStyle: DateFormatter.Style
   - 수신기의 시간 스타일입니다.
- var dateFormat: String!
   - 수신자가 사용하는 날짜 형식 문자열입니다.
- func setLocalizedDateFormatFromTemplate(String)
   - 수신자에 대해 지정된 로케일을 사용하여 템플릿에서 날짜 형식을 설정합니다.
   - locale설정후 사용 가능!
- class func dateFormat(fromTemplate: String, options: Int, locale: Locale?) -> String?
   - 지정된 로케일에 적절하게 배열된 지정된 날짜 형식 구성 요소를 나타내는 현지화된 날짜 형식 문자열을 반환합니다.
- var formattingContext: Formatter.Context
   - 날짜 형식을 지정할 때 사용되는 대문자 형식 지정 컨텍스트입니다.

## 속성 관리
- var calendar: Calendar!
   - 받는 사람을 위한 달력입니다.
- var defaultDate: Date?
   - 수신자의 기본 날짜입니다.
- var locale: Locale!
   - 수신기의 로케일.
- var timeZone: TimeZone!
   - 수신기의 시간대입니다.
- var twoDigitStartDate: Date?
   - 두 자리 연도 지정자로 표시할 수 있는 가장 빠른 날짜입니다.
- var gregorianStartDate: Date?
   - 수신자의 그레고리력 시작 날짜입니다.

## Dataformatter 를 구현해보자!
```swift
extension DateFormatter {
    func setUpDate(from timeInterval: Double) -> String {
        Self().dateStyle = .long
        Self().locale = .autoupdatingCurrent
        
        let date = Date(timeIntervalSinceReferenceDate: timeInterval)
        let result = Self().string(from: date)
        
        return result
    }
}
```
위의 메서드를 Dataformatter를 extension해서 메서드를 구현해도되고 DateFormatter를 사용하는 곳에 메서드를 만들어도되지만 DateFormatter를 여러곳에서 사용하고 메서드를 사용할때마다 DataFormatter 인스턴스를 초기화하는걸 방지하기위해 아래와 같이 만들어주었다!

```swift
struct Formatter {
    static private let dateFormatter = DateFormatter()
    
    private init() { }
    
    static func setUpDate(from timeInterval: Double) -> String {
        dateFormatter.dateStyle = .long
        dateFormatter.locale = .autoupdatingCurrent
        
        let date = Date(timeIntervalSinceReferenceDate: timeInterval)
        let result = dateFormatter.string(from: date)
        
        return result
    }
    
    static func getDate(from dateString: String) -> Date? {
        dateFormatter.date(from: dateString)
    }
    
    static func getCurrentDate() -> String {
        setUpDate(from: Date().timeIntervalSinceReferenceDate)
    }
}
```

### dateFormat 속성을 사용해 특정 형태로 변경하는 방법

- 현재 시간을 특정 형태로 변형하기 
```swift
func DateToString() -> String{
    let current = Date()
    
    //한국 시간으로 표시
    dateFormatter.locale = Locale(identifier: "ko_kr")
    dateFormatter.timeZone = TimeZone(abbreviation: "KST")
    //형태 변환
    dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
    
    return dateFormatter.string(from: current)
}
    //결과 "2022-06-24 02:06:26"
```
일단 Date() 객체를 생성하면 현재 년도와 월, 일, 시간, 분, 초 가 나오게 된다.

Local과 TimeZone을 위와 같이 해주면 한국 시간으로 나오게 된다.

그후 위에서 만들어둔 DateFormater의 인스턴스의 속성을 변경하여 한국시간으로 반환할수있도록 하는데
dateFormat속성을 변경하여 원하는 특정 형태로 만들어줄수있다!

이때 사용할수있는 형식들은 다음과 같다!
![](https://i.imgur.com/r1hkQmf.png)

위의 시간을 나타내는 H 는 대소문자를 구분하는데 대문자는 24시간 형태로 표기되며 소문자는 12시간 형태로 표기된다!

예를들면 대문자일경우 14시 소문자일경우 2시 처럼 말이다!

- String을 Date 타입으로 변경하는 방법
```swift
func StringToDate(string : String) -> Date?{
    dateFormatter.dateFormat = "yyyy-MM-dd"
    
    return dateFormatter.date(from: string)
}

StringToDate(string: "2022-06-24")
//결과 "June 24, 2022 at 12:00 AM"
```

- 시간을 추가 하는 방법 
```swift
func oneHourPlus() -> String{
    
    let current = Date()
    let oneHourLater = current.addingTimeInterval(+3600)
    
    dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
    
    return dateFormatter.string(from: oneHourLater)
}
    //결과 2022-06-24 02:32:26
```

```
**TimeInterval은 초를 의미한다.**
- 60 - 1분
- 3600 - 1시간
- 86400 - 24시간 하루
```

 - DateFormatter에 오전 오후를 붙이는 방법
```swift
func makeAMPM() -> String{
    
    let current = Date()
    
    //한국 시간으로 표시
    dateFormatter.locale = Locale(identifier: "ko_kr")
    dateFormatter.timeZone = TimeZone(abbreviation: "KST")
    //형태 변환
    dateFormatter.dateFormat = "a hh:mm:ss"
    dateFormatter.amSymbol = "오전"
    dateFormatter.pmSymbol = "오후"
    
    return formatter.string(from: now)
    
}
    //결과 "오전 02:34:26"
```

## 참고한 문서및 자료
https://hururuek-chapchap.tistory.com/156
[DateFormatter](https://developer.apple.com/documentation/foundation/dateformatter)
