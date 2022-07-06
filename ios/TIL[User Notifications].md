# Today I Learned.

## 학습내용
# User Notifications
서버에서 사용자 장치로 사용자 대면 알림을 푸시하거나 앱에서 로컬로 생성합니다.

## Overview
사용자 대면 알림은 앱이 사용자의 기기에서 실행 중인지 여부에 관계없이 앱 사용자에게 중요한 정보를 전달합니다. 예를 들어 스포츠 앱은 사용자가 좋아하는 팀이 득점할 때 알려줄 수 있습니다. 알림은 또한 정보를 다운로드하고 인터페이스를 업데이트하도록 앱에 지시할 수 있습니다. 알림은 경고를 표시하거나 소리를 재생하거나 앱 아이콘에 배지를 지정할 수 있습니다.

![](https://i.imgur.com/7otAVgw.png)

앱에서 로컬로 알림을 생성하거나 관리하는 서버에서 원격으로 알림을 생성할 수 있습니다. 로컬 알림 의 경우 앱은 알림 콘텐츠를 생성하고 알림 전달을 트리거하는 시간 또는 위치와 같은 조건을 지정합니다. 원격 알림 의 경우 회사 서버에서 푸시 알림을 생성하고 APN(Apple 푸시 알림 서비스)에서 해당 알림을 사용자 장치로 전달합니다.

User Notifications 프레임워크를 사용하여 다음을 수행합니다.
- 앱이 지원하는 알림 유형을 정의합니다.
- 알림 유형과 연결된 사용자 지정 작업을 정의합니다.
- 배달을 위해 지역 알림을 예약합니다.
- 이미 전달된 알림을 처리합니다.
- 사용자가 선택한 작업에 응답합니다.

시스템은 적시에 로컬 및 원격 알림을 전달하기 위해 모든 시도를 하지만 전달이 보장되지는 않습니다. PushKit 프레임워크는 VoIP 및 watchOS 컴플리케이션이 사용하는 특정 유형의 알림에 대해 보다 시기 적절한 전달 메커니즘을 제공합니다.

---
## User Notifications을 사용해보자
요런놈인데 앱을 사용하지 않을때 위의 사진처럼 device에 알림을 띄워주는 프레임워크다!

앱에서 push알림이 오는것을 말하며 요게 바로 User Notifications 기능이다.

요녀석을 사용하기위해서는 다음과 같은 순서로 진행한다!
1. 유저한테 알림허락을 받아야한다. (권한을 얻어야한다.)
2. 어떨 때 알림을 받고싶은지 디자인해주자.
3. push알림 메세지를 설정해주자.
4. 알림 트리거 지정
5. 알림 요청을 하자!
6. 5번에서 했던 요청을  알림센터에 추가해주자. 

### 1. 유저한테 알림허락을 받아야한다. (권한을 얻어야한다.)
- User Notifications 프레임워크를 가져와야한다!

```swift
import UserNotifications
```

요렇게 사용하는곳에서 가져오면된다


<img src = "https://i.imgur.com/qWl17IB.png" width = "300">

요런식으로 유저한테 권한을 묻는것을 필수로 해주어야한다!

앱이 너한테 알림보내도 되겠니?? 

```swift
override func viewDidLoad() {
super.viewDidLoad()
    UNUserNotificationCenter.current()
    .requestAuthorization(
        options: [.alert,.sound,.badge], completionHandler: {didAllow,Error in })
    
    //completionHandler의 원형 : completionHandler: (Bool, Error?) -> Void)
}
```
requestAuthorization 요녀석이 앱을 사용하는 유저에게 권한을 요청하는 메서드!

option에는 UNAuthorizationOptions가 들어간다!

completionHandler는 유저가 권한을 허락했는지 안했는지 에 따라 분기처리를 해줄수있다!

Bool값을 이용해 허용됐는지 허용되지 않았는지를 가지고있는다.

```swift
override func viewDidLoad() {
super.viewDidLoad()
    UNUserNotificationCenter.current()
    .requestAuthorization(
        options: [.alert,.sound,.badge], completionHandler: {didAllow,Error in 
     print(didAllow) 
    })
```
요렇게 하면 didAllow에 true가 들어간다!
콘솔에 true가 출력됨!

### 2. 어떨 때 알림을 받고싶은지 정해주자!
<img src = "https://i.imgur.com/gevAXHe.png" width = "250">

요렇게 버튼누르면 push알림이 오도록 구현해보자! 

```swift
@IBAction func didTapPushNoti(_ sender: UIButton) {
        
    }
```
내부에 알림띄울꺼 설정해주면 되겠죠?

### 3. push알림 메세지를 설정해주자.
맨위의 예시 사진처럼 push알림이 왔을때 어떤 메세지를 보여줄지 설정해주어야한다.

UNMutableNotificationContent 요클래스를 사용해주면된다.

![](https://i.imgur.com/ZbvtGG8.png)

title, sound등등 다양한 설정이 가능하다!

```swift
@IBAction func didTapPushNoti(_ sender: UIButton) {
        let content = UNMutableNotificationContent()
        content.title = "This is title : malrang"
        content.subtitle = "This is Subtitle : UserNotifications tutorial"
        content.body = "This is Body : UserNotifications"
        content.badge = 1
    }
```
요렇게 UNMutableNotificationContent 요녀석 사용해서 만들어줬다!

badge는 아래의 요녀석!

![](https://i.imgur.com/Nvjcdzk.png)

이거 기능 안쓸거면 아까 ViewDidLoad에서 설정한 options: 에 .badge를 제거하면 알림표시 기능이 사라진다!

### 4. 알림 트리거 지정
그럼 위에서 버튼을 누를경우 push알림이 오도록 하려고 Action 메서드 내부에 UNMutableNotificationContent 을 만들어주었지만 아직 UserNotifications을 언제 작동할건지 트리거를 만들어준 상태는 아니다!

트리거가 호출되면 어떤push를 주겠다! 까지만 만들어준거다!

특정시간, 시간간격, 위치변경을 기반으로 트리거를 설정할 수 있다.

위의 4개의 트리거 설정을 할 수 있다.

TimeInterval을 사용해 알림이오도록 구현해보자!

버튼 누르면 몇초뒤에 알림오도록 하는거다!(Calendar 로 도 가능하다.)

```swift
let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 5, repeats:false)
```
timeInterval 에는 초단위 값이 들어간다.

### 5. 알림 요청을 하자!
알림 요청은  UNNotificationRequest 클래스를 이용한다.

UNNotificationRequest에는 아까 우리가 만들었던 content와 트리거를 넘겨주게 된다.

```swift
let request = UNNotificationRequest(identifier: "timerdone", content: content, trigger: trigger)
```
UNNotificationRequest 를 초기화할때는 식별자가 필요한데 알림이 여러개일때 구분 알림들을 구분할수 있도록 하는 식별자다.

보류중인 알림 요청 또는 전달 된 알림을 바꾸거나 제거하는 데 사용할 수 있다.

### 6. 5번에서 했던 요청을 알림센터에 추가해주자. 
UNUserNotificationCenter 클래스를 사용해서 예약할수있다.

UNUserNotificationCenter는 알림요청을 처리해주는 센터 같은 역할이며 요청된 알림을 언제 처리해줄것인지 예약할수 있게 되는것이다.

유저가 알림을 받겠다고 허용한경우 UNNotificationRequest는 UNNotification을 만드는데 "사용"되며, 사용자에게 알려지게 된다.

```swift
UNUserNotificationCenter.current().add(request, withCompletionHandler: nil)
```
.add 메서드를 사용해서 아까 만든 request를 파라미터로 넣어준다!

그럼이제 버튼을 누르면 요렇게 push알람이 온다!(앱이 화면에 foreground 상태일때는 알림이 오지 않는다.)
![](https://i.imgur.com/HbR7rCd.png)

지금까지의 코드!
```swift
class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        UNUserNotificationCenter.current()
            .requestAuthorization(
                options: [.alert,.sound,.badge],
                completionHandler: {didAllow,Error in })
    }
    
    
    @IBAction func didTapPushNoti(_ sender: UIButton) {
        let content = UNMutableNotificationContent()
        content.title = "This is title : malrang"
        content.subtitle = "This is Subtitle : UserNotifications tutorial"
        content.body = "This is Body : UserNotifications"
        content.badge = 1
        
        let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 2, repeats:false)
        let request = UNNotificationRequest(identifier: "timerdone", content: content, trigger: trigger)
        UNUserNotificationCenter.current().add(request, withCompletionHandler: nil)
    }
}
```

### UNCalendarNotificationTrigger
요녀석은 날짜와 시간을 지정해 Push알림을 설정하는놈이다!

사용법은 위의 녀석과 비슷하다.

```swift
    let date = Date(timeIntervalSinceNow: 70)
    var dateCompenents = Calendar.current.dateComponents([.year, .month, .day, .hour, .minute, .second], from: date)
            
    let calendartrigger = UNCalendarNotificationTrigger(dateMatching: dateCompenents, repeats: true)
            
            
    let TimeIntervalTrigger = UNTimeIntervalNotificationTrigger(timeInterval: 0.1, repeats: false)
            
    let request = UNNotificationRequest(identifier: "\(index)timerdone", content: content, trigger: TimeIntervalTrigger)
            
    UNUserNotificationCenter.current().add(request, withCompletionHandler: nil)
```

## 참고한 문서및 자료
https://developer.apple.com/documentation/usernotifications/
https://developer.apple.com/documentation/usernotifications/scheduling_a_notification_locally_from_your_app
https://zeddios.tistory.com/157
https://developer.apple.com/documentation/usernotifications/
