# Today I Learned

# **학습내용**

## IBAction, IBOutlet의 역할

StoryBoard와의 연결고리를 담당한다.
변수나 메서드를 정의 할때  @IBAction 또는 @IBOutlet 키워드를 통해 StoryBoard에서 버튼이나 레이블같은 컴포넌트와 연결이 가능하다.
IBAction은 Event가 일어난 경우 호출되는 Action을 정의해둔 것이고, IBOutlet은 값에 접근하기위한 변수라고 보면 편할 것 같다.

### IBAction, IBOutlet의 정의
Action은 입력이 들어왔을때 어떤 행동을 할 지를 나타내고 Outlet은 데이터를 가져오는 것이다. 
앞에 있는 IB는 Interface Builder의 약자이다. 즉 IBAction은 Interface Builder를 통해 받아온 정보로 Action을 수행하겠다는 의미.

### @의 의미
@는 컴파일러에게 어떤 속성을 가지고있다고 전하는 역할을 하는 예약어이다. 
컴파일러에게 @가 붙은 명령어에 대해 어떤 attribute가 부여되었음을 말한다. 아래의 예시처럼 속성이 부여된다.
> @IBAction // Interface Builder와 연결된 Action이다.
@UIApplicationMain // App의 Main이 여기에 있다.

따라서 @IBAction 의 속성이 func의 정의 앞에 붙어있다면, 이 함수는 Interface Builder에서 사용될 수 있고 UI로 연결이 가능하다는 의미를 가진다.

### attribute 란 ?
시간날때 한번 찾아보자…

### 각종 UI Element
UI 중에서 어떤 항목을 가리키도록 설정할것인가를 의미한다.
- UIButton
- UILabel

여기에서 Action이 호출되는 Event를 설정할 수 없는 항목의 경우, @IBAction 에 해당하는 항목을 가리킬 수 없다. UITextView의 경우, Outlet으로 데이터 ( text ) 는 옮길 수 있어도, Event를 생성할 수 있는 장치가 없기에 @IBAction으로 연결이 불가능하다.

## Notification-Center
직역 하면 알림센터 라 할수있겠다.
알림을 통해 액션을 통제 하는 역할을 한다.

`Notification Center`라는 객체를 통해서 이벤트들의 발생 여부를 옵저버를 등록한 객체들에게 `Notification`을 전달 하는 방식으로 사용한다. 

> NotificationCenter: 액션 수행되었어요~ 담당자들 일해주세요~
Vc1: 네~
Vc2: 넵

- 방송국? 같은 개념 인것같다.
- 구독을 신청한 사람들에게 정보 를 전달해주는느낌?

이녀석을 사용하기 위해선 중간 매개체가 필요하다 위의 예시로 들면 방송국을 만들어야 한다.
방송국이 있어야 a가 방송국에다 정보 퍼트려 달라고 요청을할수있으니 당연하다.

여기서 방법은 2가지 있을수 있다.
- NotificationCenter 를 만들어 사용한다.
- NotificationCenter.default 를 사용한다.

NotificationCenter.default 는 swift 어딘가? 에 정의 되어있어
따로 NotificationCenter 을 만들지 않아도 사용할수 있게끔 정의 되어있는것 같다.(전역 변수같은느낌)

그런고로 클래스 내부에서 만 작동하는 NotificationCenter 을 만들고 싶다면 NotificationCenter 을 만들어서 클래스 내부에서 인스턴트를 만들어 사용할수도 있고

모든 클래스에서 사용 하고 싶다면 NotificationCenter.default 를 사용할수도 있을것 같다.

아래의 예시 에서는 NotificationCente.default 를 사용한 예시 이다.

하나의 객체 내부에서만 사용한 예시
```swift
import UIKit

struct Registrant {
    let name: String
    let phone: String
}

class ViewController: UIViewController {
    
    @IBOutlet weak var nameTextField: UITextField!
    @IBOutlet weak var phoneNumberTextField: UITextField!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var phoneNumberLabel: UILabel!
    
    var registrantList: [Registrant] = []
    var listCount: Int = 0
    
    override func viewDidLoad() {
        super.viewDidLoad()
        //register 라는 이름을 가진 놈들한테 연락이오면 어떤 걸 실행할건지 selector 로 지정해줄수있음
        NotificationCenter.default.addObserver(
            self,
            selector: #selector(tapRegisterButton),
            name: Notification.Name("register"),
            object: nil
        )
    }
    
    @IBAction func hitRegisterButton(_ sender: Any) {
        register()
        
        // NotificationCenter로 이벤트를 전송 post
        // userInfo 의 정보를 NotificationCenter 으로 보냄
        NotificationCenter.default.post(
            name: Notification.Name("register"),
            object: nil,
            userInfo: [listCount: registrantList.last ?? "값없음"]
        )
    }
    
    func register() {
        listCount += 1
        guard let nameText = nameTextField.text,
              let phoneText = phoneNumberTextField.text
        else {
            return
        }
        let registrant = Registrant(name: nameText, phone: phoneText)
        registrantList.append(registrant)
    }
    
    
    @objc func tapRegisterButton(_ notification: Notification) {
        guard let registrant = notification.userInfo?[listCount] as? Registrant else {
            return
        }
        nameLabel.text = registrant.name
        phoneNumberLabel.text = registrant.phone
    }
}
```
hitRegisterButton() 메서드를 실행 시킬경우 이름과 전화번호를 갖는Registrant타입의 인스턴스를 생성후 registrantList 에 추가하도록 되어있다.

그후 NotificationCenter 에 userInfo 를 전달 해준다.

NotificationCenter에서는 selector 에 작성한 메서드를 실행 시키게되고 해당 메서드에 userInfo 도 같이 전달해준다.

selector 로 인해 실행된 tapRegisterButton 메서드는 매개변수로 Notification 타입 을 받아 userInfo 의 데이터를 사용할수 있게된다.

userInfo 를 통해 전달받은 데이터를 가공하여 View에 표시되는 Laber.text 값을 변경줄수 있게 되었다.
