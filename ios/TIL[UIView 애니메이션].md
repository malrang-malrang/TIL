# Today I Learned.

## 학습내용
# UIView 애니메이션을 구현해보자!

# animate
>![](https://i.imgur.com/LsPtloL.png)
>
>지정된 기간, 지연, 옵션 및 완료 핸들러를 사용하여 하나 이상의 보기에 대한 변경 사항을 애니메이션으로 만듭니다.
>
>## Declaration
>```swift
>class func animate(withDuration duration: TimeInterval, 
>             delay: TimeInterval, 
>           options: UIView.AnimationOptions = [], 
>        animations: @escaping () -> Void, 
>        completion: ((Bool) -> Void)? = nil)
>```
>## Parameters
>
>- duration
>   - 애니메이션의 총 지속 시간(초 단위). 음수 또는 0 을 지정하면 애니메이션 없이 변경됩니다.
>   
>- delay
>   - 애니메이션을 시작하기 전에 대기할 시간(초 단위로 측정).
>
>- options
>   - 애니메이션을 수행할 방법을 나타내는 옵션 마스크입니다.
>
>- animations
>   - 뷰에 커밋할 변경 사항이 포함된 블록 개체입니다. 일반적으로 이 블록 내부에서 메서드를 한 번 이상 호출합니다. 전체 기간 동안 변경 사항을 적용하려면 보기 값을 직접 변경할 수도 있습니다. 이 블록은 매개변수를 사용하지 않으며 반환 값도 없습니다.매개변수에 nil 사용 불가
>   
>- completion
>   - 애니메이션 시퀀스가 끝날 때 실행할 블록 개체입니다. 이 블록에는 반환 값이 없으며 완료 핸들러가 호출되기 전에 애니메이션이 완료되었는지 여부를 나타내는 단일 부울 인수를 사용합니다. 애니메이션의 지속 시간이 0이면 이 블록은 다음 런 루프 사이클의 시작 부분에서 수행됩니다. nil이어도된다.
>
>## Discussion
>이 메서드는 뷰에서 수행할 애니메이션 세트를 시작합니다. 매개변수 의 블록 개체 animations에는 하나 이상의 보기 속성에 애니메이션을 적용하는 코드가 포함되어 있습니다.
>
>애니메이션 중에는 애니메이션 중인 보기에 대해 사용자 상호 작용이 일시적으로 비활성화됩니다. (iOS 5 이전에는 전체 응용 프로그램에 대해 사용자 상호 작용이 비활성화되었습니다.) 사용자가 보기와 상호 작용할 수 있도록 하려면 매개 변수에 상수를 포함합니다 .allowUserInteractionoptions
>
>## 예시 코드
>```swift
>@IBOutlet weak var malrangImageView: UIImageView!
>
>override func viewDidLoad() {
>    super.viewDidLoad()
>
>}
>
>@IBAction func didTapButton(_ sender: Any) {
>        UIView.animate(withDuration: 1, delay: 0, options: .repeat) {
>            self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>        } completion: { (_) in
>            
>        }
>    }
>```
>CGAffineTransform 에대한 설명은 아래에 작성되어있으니 일단은 설명만 보고 넘어가자!
>
>위의 예시코드 설명을 하자면 1초동안 애니메이션을 실행할건데 딜레이 없이(바로시작) 옵션은 repeat(반복) 으로 애니메이션을 실행할거야 라는뜻이다.
> 
>내부로직은 버튼을 누르면 malrangImageView 가 x축이 5만큼 이동한다는 뜻이다.
>
>실행되는 모습은 1초 동안 애니메이션을 실행하고 x축을 5만큼 이동한다고 했으니 말그대로 1초동안 x축으로 5만큼 천천히 이동하게된다.
>
>그후 애니메이션이 실행이 종료되면 어떤 행동을 지정할것인지는 정하지 않았기 때문에 completion 이 비어있는 모습이다.
>
> 위의 설명에서 nil 이 가능하다 했으니 제거해도 무방하다!
>```swift
>@IBAction func didTapButton(_ sender: Any) {
>        UIView.animate(withDuration: 0.1, delay: 0, options: .repeat) {
>            self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>        }
>    }
>```
>애니메이션의 옵션은 너무많으니 공식문서에서 확인하자!
>[애니메이션 옵션 공식문서](https://developer.apple.com/documentation/uikit/uiview/animationoptions)

# animateKeyframes
>![](https://i.imgur.com/sGxOB89.png)
>
>현재 보기에 대한 키프레임 기반 애니메이션을 설정하는 데 사용할 수 있는 애니메이션 블록 개체를 만듭니다.
>
>애니메이션이 여러개 이어서 동작해야할때 사용한다.
>
>## Declaration
>```swift
>class func animateKeyframes(withDuration duration: TimeInterval, 
>                      delay: TimeInterval, 
>                    options: UIView.KeyframeAnimationOptions = [], 
>                 animations: @escaping () -> Void, 
>                 completion: ((Bool) -> Void)? = nil)
>```
>
>## Parameters
>- duration
>   - 애니메이션의 총 지속 시간(초 단위). 음수 또는 0 을 지정하면 애니메이션 없이 변경됩니다.
>   
>- delay
>   - 애니메이션을 시작하기 전에 대기할 시간(초)을 지정합니다.
>
>- options
>   - 애니메이션을 수행할 방법을 나타내는 옵션 마스크입니다. 
>   
>- animations
>   - 뷰에 커밋할 변경 사항이 포함된 블록 개체입니다. 일반적으로 이 블록 내부에서 메서드를 한 번 이상 호출합니다. 전체 기간 동안 변경 사항을 적용하려면 보기 값을 직접 변경할 수도 있습니다. 이 블록은 매개변수를 사용하지 않으며 반환 값도 없습니다.매개변수에 nil 사용 불가
>   
>- completion
>   - 애니메이션 시퀀스가 끝날 때 실행할 블록 개체입니다. 이 블록에는 반환 값이 없으며 완료 핸들러가 호출되기 전에 애니메이션이 완료되었는지 여부를 나타내는 단일 부울 인수를 사용합니다. 애니메이션의 지속 시간이 0이면 이 블록은 다음 런 루프 사이클의 시작 부분에서 수행됩니다. nil이어도된다.
>
>## Discussion
>```
>동시성 참고사항
>이 페이지에 표시된 대로 완료 핸들러를 사용하여 동기 코드에서 
>이 메서드를 호출하거나 다음 선언이 있는 비동기 메서드로 호출할 수 있습니다.
>```
>```swift
>class func animateKeyframes(withDuration duration: >TimeInterval, delay: TimeInterval, options: >UIView.KeyframeAnimationOptions = [], animations: >@escaping () -> Void) async -> Bool
>```
>
>이 메서드는 키프레임 기반 애니메이션을 설정하는 데 사용할 수 있는 애니메이션 블록을 만듭니다. 키프레임 자체는 이 방법을 사용하여 만드는 초기 애니메이션 블록의 일부가 아닙니다. 블록 내 에서 메서드를 한 번 이상 animations호출하여 키프레임 시간 및 애니메이션 데이터를 추가해야 합니다 . 키프레임을 추가하면 지정한 시간에 애니메이션이 현재 값에서 첫 번째 키프레임 값, 다음 키프레임 값 등으로 보기에 애니메이션을 적용합니다.addKeyframe(withRelativeStartTime:relativeDuration:animations:)
>
>블록에 키프레임을 추가하지 않으면 animations애니메이션이 표준 애니메이션 블록처럼 처음부터 끝까지 진행됩니다. 즉, 시스템은 현재 보기 값에서 지정된 duration.
>
>## 예시코드
>```swift
>@IBAction func didTapButton(_ sender: Any) {
>        UIView.animateKeyframes(withDuration: 2, delay: 0, options: .autoreverse) {
>            UIView.addKeyframe(withRelativeStartTime: 0, relativeDuration: 0.5) {
>                self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>            }
>            UIView.addKeyframe(withRelativeStartTime: 0.5, relativeDuration: 0.5) {
>                self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>            }
>        }
>    }
>```
>예시 코드 설명을 하자면 Keyframes 내부에 keyframe 들을 추가하는 느낌? 이다.
>
>하지만 이때 중요한게 시간을 설정해주어야 한다.
>Keyframes 에 전체 애니메이션이 실행될 속도를 설정한후
>내부의 keyframe 들이 설정한 시간을 나눠써야한다.
>
>위의 코드처럼 애니메이션이 실행되는 시간을 총 2초로 설정하게되면 내부의 keyframe 들이 2초를 나누어 써야하는데 어떻게 설정을해야하는지는 매개변수로 설정해줄수있다.
>
>그럼 addKeyFrame 메서드를 한번 봐보자
>
># addKeyframe
>
>키프레임 애니메이션의 단일 프레임에 대한 타이밍 및 애니메이션 값을 지정합니다.
>
>## Declaration
>```swift
>class func addKeyframe(withRelativeStartTime frameStartTime: Double, 
>      relativeDuration frameDuration: Double, 
>            animations: @escaping () -> Void)
>```
>
>## Parameters
>**frameStartTime**
지정된 애니메이션을 시작할 시간입니다.0~1 까지 범위 에 값이 있어야 합니다. 여기서 0은 전체 애니메이션의 시작을 나타내고 1은 전체 애니메이션 끝을 나타냅니다. 예를 들어 지속 시간이 2초인 애니메이션의 경우 시작 시간을 0.5로 지정 하면 전체 애니메이션이 시작된 후 1초 후에 애니메이션이 실행되기 시작합니다.
>
>**frameDuration**
지정된 값으로 애니메이션을 적용하는 데 걸리는 시간입니다. 0값을 지정하면 블록 에 설정한 모든 속성이 animations지정된 시작 시간에 즉시 업데이트됩니다. 0이 아닌 값을 지정하면 속성이 해당 시간 동안 애니메이션됩니다. 예를 들어 지속 시간이 2초인 애니메이션의 경우 지속 시간을 0.5 로 지정 하면 애니메이션 지속 시간이 1초가 됩니다.
>
>**animations**
수행하려는 애니메이션이 포함된 블록 개체입니다. 여기에서 보기 계층 구조에서 보기의 애니메이션 가능한 속성을 프로그래밍 방식으로 변경합니다. 이 블록은 매개변수를 사용하지 않으며 반환 값도 없습니다. 이 매개변수는 nil이 아니어야 합니다.
>
>쉽게 말하면 
>frameStartTime 요녀석은 전체 애니메이션의 시간중 몇초부터 시작할것인지 정해주는 파라미터.
>frameDuration 요녀석은 Keyframes 에 지정된 전체 애니메이션 실행 시간 에서 얼만큼 애니메이션을 실행시킬것인지를 말한다!
>
>다시 위에있던 예제 코드를 가져와보자
>```swift
>@IBAction func didTapButton(_ sender: Any) {
>        UIView.animateKeyframes(withDuration: 2, delay: 0, options: .autoreverse) {
>            UIView.addKeyframe(withRelativeStartTime: 0, relativeDuration: 0.5) {
>                self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>            }
>            UIView.addKeyframe(withRelativeStartTime: 0.5, relativeDuration: 0.5) {
>                self.malrangImageView.transform = CGAffineTransform(translationX: 5, y: 0)
>            }
>        }
>    }
>```
>
>전체 애니메이션이 실행되는 시간은 2초! = withDuration
>
>그중에 처음으로 실행될 애니메이션이 실행될시간은 0이니까 바로 시작할거야! = withRelativeStartTime
>
>그럼 처음 애니메이션이 몇초동안 실행될거야?? 0.5 니까 전체 애니메이션 의 시간2초 의 50% 즉 1초가 된다! = relativeDuration
>
>그후 Keyframes 에 등록된 두번째 애니메이션은 언제 실행될거야? 0.5 니까 1초 부터 시작하겠죠?
>애니메이션이 몇초동안 실행될거야? 0.5니깐 1초간 실행하겠죠?
>
>전체 애니메이션 2초간 실행한다고 했으니 알뜰하게 잘썼군요!
>

# CGAffineTransform
>![](https://i.imgur.com/4YfDGwb.png)
>
>2D 그래픽을 그리는 데 사용하기 위한 아핀 변환 행렬입니다.
>
>요녀석 구조체인데 뷰의 프레임을 계산하지 않고 2D 그래픽을 그릴수 있다.
>
>## Declaration
>```swift
>struct CGAffineTransform
>```
>
>## OverView
>객체를 회전 크키조절, 변환, 기울기 설정할때도 사용된다!
>
>CGAffineTransform 은 3x3 행렬로 표시된다.
>
>![](https://i.imgur.com/CkpG43w.png)
>
>세 번째 열은 항상 (0,0,1)이므로 데이터 구조에는 처음 두 열에 대한 값만 포함됩니다.CGAffineTransform
>
>개념적으로 아핀 변환은 도면의 각 점(x,y)을 나타내는 행 벡터에 이 행렬을 곱하여 해당 점(x',y')을 나타내는 벡터를 생성합니다.
>
>![](https://i.imgur.com/AN858ft.png)
>
>3x3 행렬이 주어지면 다음 방정식을 사용하여 한 좌표계의 점(x, y)을 다른 좌표계의 결과 점(x',y')으로 변환합니다.
>
>![](https://i.imgur.com/rFOAD4R.png)
>
>따라서 행렬은 두 좌표계를 "연결"합니다. 즉, 한 좌표계의 점이 다른 좌표계의 점에 매핑되는 방법을 지정합니다.
>
>일반적으로 아핀 변환을 직접 만들 필요는 없습니다. 예를 들어 크기가 조정되거나 회전된 개체만 그리려면 그렇게 하기 위해 아핀 변환을 구성할 필요가 없습니다. 이동, 크기 조정 또는 회전에 관계없이 그림을 조작하는 가장 직접적인 방법은 각각 , 또는 , 또는 함수 를 호출하는 것 입니다. 나중에 재사용하려는 경우에만 일반적으로 아핀 변환을 생성해야 합니다.
>
>![](https://i.imgur.com/ZYo573V.png)
>
>사실 동작원리는 이해가 안가지만...먼훗날 이해하게 되겠지..?
>
>무튼 사용법은 알겠다 
>
>## 예시코드 
>```swift
>// scale 뷰의 넓이와 높이를 2배로 증가시킨다.
>self.malrangImageView.transform = CGAffineTransform(scaleX: 2.0, y: 2.0)
>        
>// rotationAngle 뷰를 180도로 회전 시킨다.
>self.malrangImageView.transform = CGAffineTransform(rotationAngle: .pi)
>    
>// translation 뷰의 위치를 x 200, y 200 으로 움직인다.
>self.malrangImageView.transform = CGAffineTransform(translationX: 200, y: 200)
>```
>
>이제 각각 애니메이션을 어떻게 구현해야하는 지 알겠다. 하지만 한번에 하나씩의 애니메이션 밖에 구현 못하지 않는가??
>
>나는 동시에 여러개의 애니메이션이 일어났으면 좋겠는데!!
>회전하면서 옆으로 움직인다던지 말이다!!
>
>그럴 때는 connatenating 을 사용해 합쳐주면된다!
>```swift
>let scale = CGAffineTransform(scaleX: 2.0, y: 2.0)
>    let rotate = CGAffineTransform(rotationAngle: .pi)
>    let move = CGAffineTransform(translationX: 200, y: 200)
>    
>    let combine = scale.concatenating(rotate).concatenating(move)
>    
>    malrangImageView.transform = combine
>```
>요렇게 해주면 malrangImageView 는 가로 세로로 2배씩 커지며 180도를 회전하며 위치도 x200, y200 으로 움직이게된다! 동시에!
>


