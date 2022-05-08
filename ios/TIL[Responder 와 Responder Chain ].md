# Today I Learned.

## 학습내용
# IOS Responder 와 Responder Chain 

## Responder
>**`Responder` 는 이벤트를 핸들링하고 이벤트에 반응할 수 있는 객체다.**
>모든 `Responder` 객체는 `UIResponder` 에서 상속된 클래스들의 인스턴스다.
>
>`UIResponder` 클래스는 이벤트 핸들링을 위한 인터페이스와 `Responder`들의 기본적인 행위를 정의한다.
>
>`UIApplication`, `UIViewController`,`UIView` 객체들 을 상속받는 객체들은 모두 `Responder` 다.
>
>이벤트가 일어나면 UIKit 은 이벤트 핸들링을 위해 해당 이벤트를 앱의 `responder` 객체들에게 보내게 된다.
>
>![](https://i.imgur.com/ffTEXz4.png)
>
>### IOS 환경에서 사용자가 일으킬수 있는 이벤트의 종류
>
>| Event Type            | First Responder             | 
>| --------              | --------                    | 
>| Touch events             | 터치가 일어난 뷰                |
>| Press events             | 포커스를 가진 뷰                |
>| Shake-motion events   | UIKit이 지정한 객체 또는 직접 지정 |
>| Remote-control events | UIKit이 지정한 객체 또는 직접 지정 |
>| Editing menu messages | UIKit이 지정한 객체 또는 직접 지정 |
>
>**특정 이벤트를 핸들링하기 위해서는 `Responder` 가 해당 이벤트에 대응되는 메서드들을 오버라이드하여 구현해야한다.**
>
>### 터치 이벤트 핸들링예시
>```swift
>override func touchesBegan(_:with:) 
>override func touchesMoved(_:with:) 
>override func touchesEnded(_:with:) 
>override func touchesCancelled(_:with:) 
>```
>
>`Responder` 가 터치되었을때 위의 메서드를 오버라이드하여 구현해주면 어떤 행동을 취할것인지를 정의해줄수 있다.
>
>`Responder` 는 터치의 변화를 트래킹 하여 앱의 인터페이스를 적절히 업데이트 하기 위해 UIKit에서 제공하는 이벤트 정보를 이용한다.
>
>
>`Responder` 들은 `UIEvent`객체를 처리할 수도 있고, `input view`를 통해 커스텀 `input`을 받아들일 수도 있다.(예시: 시스템 키보드)
>앱을 사용하는 사용자가 화면의 `UITextField` 또는 `UITextView` 객체를 탭하면 해당 뷰는 `first Responder` 가 되어 `input view`를 디스플레이한다.
>
>위의 예시의 경우에서 `input view` 는 키보드를 얘기한다. 입력받을수있는 무언가를 뜻한다.
>
>커스텀 `input view` 를 생상하여 다른 `Responder`가 활성화 되었을때 만들어둔 커스텀`input view`를 디스플레이 할수도있다.
>
>예를 들어 아무 기능이없는 view 를 탭했을때 만들어둔 커스텀 `input view` 여기서는 키보드를 예시로 삼겠다.
>키보드 입력창이 올라오도록 할수있다.
>
>### `UIKit` 에 정의된 `UIResponder`
>```swift
>@available(iOS 2.0, *)
>open class UIResponder : NSObject, >UIResponderStandardEditActions {
>
>    open var next: UIResponder? { get }
>    
>    open var canBecomeFirstResponder: Bool { get } // default is NO
>    open func becomeFirstResponder() -> Bool
>
>    open var canResignFirstResponder: Bool { get } // default is YES
>    open func resignFirstResponder() -> Bool
>
>    open var isFirstResponder: Bool { get }
>    // ...
>}
>```

## The Responder Chain
>이벤트 핸들링 말고도 `UIKit Responder`들은 처리되지 않은 이벤트를 앱의 다른 파트로 전송 하는 일도 담당한다.
>
>`Responder Chain`은 `Responder` 객체들이 이벤트나 액션 메세지를 핸들링할 책임을 앱의 다른 객체들에게 전송할 수 있도록 한다.
>
>특정 정해진 `Responder` 가 이벤트를 핸들링 하지 않을 경우, 해당 `Responder`는 그 이벤트를 `Responder chain`의 다음 객체에게로 전송한다.
>
>전송받은 `Responder` 객체가 이벤트를 핸들링하지 않을 경우, 해당 `Responder` 는 그 이벤트를 `Responder chain`의 다음 객체에게로 전송한다.
>
>메세지는 처리될때까지 게속 `Responder chain`의 상위 객체들로 이동한다.
>
>최상위 객체에서도 처리되지 않을경우 앱이 해당 메세지를 버린다.
>마지막까지 이벤트를 처리를 담당해줄 `Responder` 가 없다면 이벤트를 무시하겠단 뜻이다.
>
>![](https://i.imgur.com/dh46eB0.png)
>
>### The path of an event(이벤트의 경로)
>
>일반적인 이벤트는 `Responder chain`에서 뷰(`first Responder 또는 터치된 뷰`)에서 시작해서 뷰계층을 따라 `window` 객체를 거쳐 `app` 객체에 도달할때 까지 이동한다.
>
>`UIKit`은 일정한 규칙을 사용하여 `Responder chain`을 동적으로 관리한다.
>
>이러한 규칙에 의해 어떤 객체가 다음 순서로 이벤트를 받을지가 결정된다.
>
>예를들어 뷰는 뷰의 슈퍼뷰에게로 이벤트를 전송하고, 뷰계층의 루트뷰는 루트뷰의 컨트롤러에게로 이벤트를 전송한다.
>
>### `Responder cahin`을 따라 다음 `Responder`로 전송되는 예시
>
>![](https://i.imgur.com/UBaDrec.png)
>
>- 1. `Text Field`가 이벤트를 핸들링하지 않으면 `UIKit`은 `Text Field` 의 부모인 `UIView`에게로 이벤트를 보내고 이어서 윈도우의 루트뷰에게로 보낸다.
>
>- 2. 루트뷰에서 `Responder chain`은 이벤트를 윈도우로 보내기 전에 방향을 바꾸어 루트 뷰을 소유하고있는 뷰컨트롤러로 보낸다.
>    - 뷰컨트롤러가 없는경우 `UIWindow` 로 보내게된다.(그림의 점선을 의미)
>    
>- 3.윈도우가 이벤트를 처리하지 못하면 `UIKit` 은 이벤트를 `UIApplication` 객체로 보낸다.
>    - `app Delegate`가 `UIResponder`의 인스턴스이고 `Responder chain`의 일부가 아니라면 이벤트를 `app Delegate`로 보낸다.

## The First Responder
>앱에서 많은 종류의 이벤트들을 처음으로 받는 `Responder`객체를 `First Responder` 라고 한다.
>
>`First Responder`는 앱의 이벤트를 핸들링하기 가장 적합하다고 간주하는 `Responder`객체다.
>
>앱이 이벤트를 받으면 `UIKit`이 해당 이벤트를 가장 적합한 `Responder`객체인 `First Responder`에게 전송한다.
>
>이벤트를 받기위해서는 `Responder`자신이 `First Responder`가 될수 있음을 나타내야 한다.
>
>`First Responder`가 될수 있게 하려면, `UIResponder`의 서브 클래스에서 `canBeComeFirstResponder`프로퍼티를 오버라이드 하여 `true`를 리턴하도록 해줘야한다.
>```swift
>override var canBeComeFirstResponder = true
>```
>
>이벤트 메세지들을 받는것 이외에, `Responder`는 `target`이 특정되지 않은 액션 메세지 들을 받을수도 있다.
>
>액션 메세지 들은 버튼이나 사용자가 조절 가능한 컨트롤 들 등 컨트롤들로부터 보내진다.
>
>### Determining an Event’s First Responder(이벤트의 첫 번째 응답자 결정)
>
>`UIkit`은 받은 이벤트의 종류에 따라 특정 객체를 해당 이벤트의 `First Responder`로 지정한다.
>
>| Event Type            | First Responder             | 
>| --------              | --------                    | 
>| Touch events             | 터치가 일어난 뷰                |
>| Press events             | 포커스를 가진 뷰                |
>| Shake-motion events   | UIKit이 지정한 객체 또는 직접 지정 |
>| Remote-control events | UIKit이 지정한 객체 또는 직접 지정 |
>| Editing menu messages | UIKit이 지정한 객체 또는 직접 지정 |
>
>컨트롤은 연관된 타겟 객체와 직접 액션 메세지를 이용해 소통한다.
>
>액션 메서드는 이벤트는 아니지만 `Responder chain`을 이용한다.
>컨트롤의 타겟 객체가 `nil` 일 경우 `UIKit`은 `first Responder`에서 시작하여 적절한 액션 메세지를 구현한 객체를 만날때 까지 `Responder chain`을 따라 이동한다.
>
>뷰에 있는 `Gesture recognizer`의 경우 마찬가지로 터치 등을 인식하지 못하면 `UIKit`은 뷰로 터리를 보내고 뷰도 터치를 처리하지 않을 경우 마찬가지로 `responder chain`을 따라 이벤트를 보낸다.
>
>### Determining Which Responder Contained a Touch Event(터치 이벤트가 포함된 응답자 확인)
>
>`UIKit`은 어디서 터치 이벤트가 발생했는지를 결정하기 위해 뷰 기반의 `hit-testing`을 사용한다.
>
>`UIKit`은 터치 위치를 뷰계층에 있는 뷰객체의 바운드와 비교한다.
>`UIView` 의 `hitTest(_:with:)` 메서드는 특정 터치를 포함하는 가장 깊은 서브뷰를 찾기위해 뷰 계층을 따라 이동하고, 가장 깊은 서브뷰가 터치 이벤트의 `First Responder`가 된다.
>
>**`Text Field`에서의 `hitTest`메서드**
>```swift
> override func hitTest(_ point: CGPoint, with event: >UIEvent?) -> UIView? {
>        if !isUserInteractionEnabled || isHidden || alpha <= 0.01 {
>            return nil
>        }
>        if self.point(inside: point, with: event) {
>            for subview in subviews.reversed() {
>                let convertedPoint = subview.convert(point, from: self)
>                if let hitTestView = subview.hitTest(convertedPoint, with: event) {
>                    return hitTestView
>                }
>            }
>            return self
>        }
>        return nil
>    }
>```
> 예시 코드를 보면 `subviews.reversed()` 가 있는걸 볼수 있다.
> 속해있는 뷰계층을 반대로 뒤집어서 가장 깊게있던 `TextField` 를 `First Responder` 로 지정하는것을 알수있다.
> 
>터치 이벤트가 발생하면 `UIKIt`은 `UITouch` 객체를 만들고 뷰와 연결한다.
>
>### Altering the Responder Chain(응답자 체인 변경)
>`Responder` 객체의 `next` 프로퍼티를 오버라이드 하여 `Responder chain`을 변경할수 있다.
>
>이때 `next responder` 는 오버라이드한 프로퍼티에서 반환하는 객체다.
>```swifrt
>override var next: UIResponder? {
>        return orangeView
>    }
>```
>
>많은 `UIKit` 클래스 들이 `next`프로퍼티를 오버라이드 하여 특정 객체를 반환하고 있다.
>
>- 1. `UIView`: 만약 해당뷰가 뷰컨트롤러의 루트 뷰라면, `next responder`는 뷰컨트롤러다.
>    - 루트뷰가 아니라면 `next responder`는 해당 뷰의 슈퍼뷰다.
>- 2. `UIViewController`: 만약 뷰컨트롤러의 뷰가 윈도우의 루트뷰라면 `next responder`는 `window`다. 
>    - 뷰컨트롤러가 다른 뷰컨트롤러에 의해 프레젠트 된경우`next responder` 는 `presenting` 뷰 컨트롤러다.
>- 3. `UIWindow`: 윈도우의 `next responder`는 `UIApplication` 이다.
>- 4. `UIApplication`: `UIApplication` 객체의 `next responder`는 `app delegate`다.
>    - `appDelegate`가 `UIResponder`의 인스턴스 이면서 뷰, 뷰컨트롤러, 또는 앱 객체 자신이 아닐때만 해당된다.

## 생각해볼것들

### Q1 : iOS 환경에서 사용자의 터치 이벤트를 알아채거나 제어할 수 있는 방법의 종류를 알아봅시다
- UIControl(target action 메커니즘 - event가 발생하면 지정한 target에 action message를 전달한다)
- Responder(타겟이 정해지지 않으면 Responder Chain에서 이벤트를 처리할수있는 애를 찾아 넘겨준다)

### Q2 : iOS 환경에서 사용자가 일으킬 수 있는 이벤트의 종류는?


| Event Type | First Responder | 
| -------- | -------- | 
| Touch events         | 터치가 일어난 뷰     |
| Press events         | 포커스를 가진 뷰     |
| Shake-motion events     | UIKit이 지정한 객체 또는 직접 지정     |
| Remote-control events         | UIKit이 지정한 객체 또는 직접 지정     |
| Editing menu messages    | UIKit이 지정한 객체 또는 직접 지정 |


Touch Events에 Gesture(Tap, Drag, Swipe, Pinch 등)가 포함되어 있는건가?

### Q3 : 뷰 위에 텍스트 필드가 있고 텍스트 필드 위에 탭 제스쳐 인식기가 있는 상황에서 각 상황에서 사용자가 텍스트 필드 위를 탭 했을 때 어떤어떤 객체가 어떤 메서드를 통해 반응하나요?
textfield 객체가 제스처메서드를 통해 반응
 
touchesBegan(_:with:) 
- 보기 또는 창에서 하나 이상의 새로운 터치가 발생했음을 이 개체에 알린다

touchesMoved(_:with:)
- 이벤트와 관련된 하나 이상의 터치가 변경되면 응답자에게 알린다

touchesEnded(_:with:)
- 보기 또는 창에서 하나 이상의 손가락을 들 때 응답자에게 알린다

touchesCancelled(_:with:)
- 시스템 이벤트(예: 시스템 경고)가 터치 시퀀스를 취소할 때 응답자에게 알린다

### Q4 : Responder Chain과 Gesture Recognizer는 이벤트 제어에서 상호간 상관관계일까요? 별개관계일까요? 그렇게 생각한 이유는 무엇인가요? 
marisol: Gesture Recognizer가 이벤트를 제일 먼저 수신하고, 인식하지 못하는 경우 view로 이벤트를 보내고, 그 뷰도 이벤트를 처리하지 못하는 경우 Responder Chain을 타는 것 같다 -> 둘이 상호작용이 없어서 별개?? 🤔 
동의(malrang, minseong)

### 참고 문서 및 블로그
[Breadcrumbs](https://seizze.github.io/2019/11/26/iOS%EC%9D%98-Responder%EC%99%80-Responder-Chain-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0.html)
[Using Responders and the Responder Chain to Handle Events](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)
[UIResponder](https://developer.apple.com/documentation/uikit/uiresponder)
[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html)
