# Today I Learned.

## 학습내용
# Date Picker 

아래의 글은 IOS 14.1 버젼의 내용이며 14버젼 이하는 Preferred Style이 없다.

아이폰에서 날짜를 고를 때 자주 마주치게되는 인터페이스 중 하나인 DatePicker!
유저에게 날짜를 고르게 하고 고른 날짜 데이터를 가져올수있게 하는것이다.

![](https://i.imgur.com/NhrckDu.png)

요런 녀석을 앱을 사용하면서 종종 봤을텐데 요녀석이다!

Date Picker는 Delegate나 DataSource는 따로 없다. 

사용은 어렵지 않다!
```swift
private let datePicker = UIDatePicker()

private func setAttributes() {
        datePicker.preferredDatePickerStyle = .automatic
        datePicker.datePickerMode = .dateAndTime
        datePicker.locale = Locale(identifier: "ko-KR")
        datePicker.timeZone = .autoupdatingCurrent
        datePicker.addTarget(self, action: #selector(handleDatePicker(_:)), for: .valueChanged)
    }

@objc private func handleDatePicker(_ sender: UIDatePicker) {
        let dateformatter = DateFormatter()
        dateformatter.dateStyle = .none
        dateformatter.timeStyle = .short
        let date = dateformatter.string(from: datePicker.date)
    }
```

DatePicker 를 Target Action 으로 구현하고자 할 때는.valueChanged 옵션을 써야한다.
atePicker 는 Intrinsic Size 로 Height 값을 갖고 있으므로 따로 높이를 지정해 줄 필요가 없다.

하지만 DatePicker를 통해 전달된 Date값은 설정해둔 옵션과 다르게 전달된다. DatePicker는 기본적으로 표시되는 시간이 GMT Format을 따르기 때문인데 위와같이 DateFormatter를 사용해 해결할수 있다!

DateFormatter 에는 여러가지 스타일이 존재하는데 원하는걸 선택하면 된다!

## Preferred Style의 종류
- **1. automatic**
    - 요녀석은 현재 뷰에 가장 알맞은 스타일을 선택해준다.
    - 아무것도 설정하지않으면 요녀석이 defult값이다.

- **2. wheel**
    ![](https://i.imgur.com/jt17s2V.png)
    - 휠을 돌려서 날짜를 선택할수있다.

- **3. compact**
    ![](https://i.imgur.com/Dx6lS6a.png)
    - 시간이 적혀있는 label을 유저가 탭하면 원하는 값으로 수정할 수 있다.
    
- **4. inline**
    ![](https://i.imgur.com/eXOqttZ.png)
    - 달력 모양이 표기되며 원하는날짜를 택할수있다.

## DatePicker의 Mode의 종류
- 1. counDownTimer
    ![](https://i.imgur.com/APD2DLO.png)
- 2. date
    ![](https://i.imgur.com/ZnePOOX.png)
- 3. time
    ![](https://i.imgur.com/qnwbsz7.png)
- 4. dateAndTime
    ![](https://i.imgur.com/uwAZb7Q.png)

## Locale
.locale 은 DatePicker 에 사용하는 국가를 입력할 수 있다.
아무런 설정도 건드리지 않으면 아이폰 기본설정을 사용한다.

한국으로 설정이 필요하다면 요렇게!

```swift
datePicker.locale = Locale(identifier: "ko-KR")
```

## minuteInterval
.minuteInterval 은 사용자가 스크롤을 돌려 시간을 설정할 때 나타나는 분 단위 간격을 조절할 수 있다.
기본값은 1분으로 되어있다.
최대 30분까지 입력이 가능합니다.
이 설정을 사용할 때는 1시간은 60분이니까 60의 약수 내에서 입력하도록 한다.

```swift
datePicker.minuteInterval = 5
```
이렇게 설정하면 분 단위 설정이 5분 간격으로 가능하다.

## Date
최초에 선택되어 있는 날짜를 설정할 수가 있는 프로퍼티!
DatePicker 의 Mode 가 .countDownTimer 로 설정되어 있을 때는 작동하지 않는다.

```swift
datePicker.date = Date(timeIntervalSinceNow: -3600 * 24 * 3)
```
이렇게 코드를 입력하면 DatePicker 가 현재 날짜를 기준으로 3일 전의 날짜로 선택되어 시작한다.

## minimumDate, maximumDate
사용자가 선택할 수 있는 날짜나 시간을 한정할 수 있게 도와주는 프로퍼티.

.wheel 모드에서는 사용자가 우리가 지정해놓은 한계 이상으로 스크롤을 했을 경우 처음 기본값 상태로 되돌아오게 됩니다.
.compact 나 .inline 모드에서는 선택할 수 있는 날짜가 연한 회색으로 비활성화되는 방식으로 사용자에게 이 날짜는 선택할 수 없다는 것을 직관적으로 알 수 있게 한다.

```swift
var components = DateComponents()
components.day = 10
let maxDate = Calendar.autoupdatingCurrent.date(byAdding: components, to: Date())
components.day = -10
let minDate = Calendar.autoupdatingCurrent.date(byAdding: components, to: Date())

datePicker.maximumDate = maxDate
datePicker.minimumDate = minDate
```
위 코드를 입력하면 현재 날짜를 기준으로 앞뒤 10일까지만 선택이 가능해집니다.

## 참고한 문서 및 자료 
https://kasroid.github.io/posts/ios/20201030-uikit-date-picker/
https://zeddios.tistory.com/291
