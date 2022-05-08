# Today I Learned

# **학습내용**
## Programmatically Creating Constraints
오토레이아웃의 조건 들을 코드로 작성해보자!

![](https://i.imgur.com/sxQJyna.png)

![](https://i.imgur.com/OZxXZ59.png)

위의 조건들을 코드로 작성할수 있도록 해보자!
- 버튼은 safeArea 에서 좌우 20 포인트만큼 떨어져있다.
- 버튼의 y축은 superView에서 최소 20 만큼 떨어져있다.(우선도 높음)
- 버튼의 y축은 safeArea에서 0 만큼 떨어져있다.(우선도 중간)

### Layout Anchors(레이아웃 앵커)
- 가장 간단하고 안전한 방법중 하나이다.
- 코드가 간결해진다.
- 오류체크가 간편하다.

```swift
class AnchorViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        let button = UIButton()
        button.setTitle("Button", for: UIControl.State.normal)
        button.setTitleColor(.white, for: .normal)
        button.backgroundColor = .systemGreen
        view.addSubview(button)
        
        button.translatesAutoresizingMaskIntoConstraints = false
        
        let safeArea = view.safeAreaLayoutGuide
        
        button.leadingAnchor.constraint(equalTo: safeArea.leadingAnchor, constant: 16 ).isActive = true
        button.trailingAnchor.constraint(equalTo: safeArea.trailingAnchor, constant: -16 ).isActive = true
        let safeBottomAnchor = button.bottomAnchor.constraint(equalTo: safeArea.bottomAnchor)
        safeBottomAnchor.isActive = true
        safeBottomAnchor.priority = .defaultHigh
        
        let viewBottomAnchor = button.bottomAnchor.constraint(lessThanOrEqualTo: view.bottomAnchor, constant: -20)
        viewBottomAnchor.isActive = true
    }
}
```
- .isActive 는 활성화 시켜줄것을 의미한다 Bool 값을 가질수있다. 조건은 변수에 할당해준뒤 해당변수에.isActive 에 Bool 값을 넣어줘도 가능하다.
- 우선도의 값을 직접 작성하고 싶을때는 이와 같은 방법으로`safeBottonAnchor.priority = .init(999)`
-  상관관계 에 대해 생각해보아야한다. 양수인지 음수인지, Equal 이 아니라면 어떤것이 들어가야하는지
- First, Second 아이템의 관계를 생각해보아야 한다.

### LayoutConstraint Class
인스턴스를 직접 만드는 방법!

```swift
class ConstraintViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let button = UIButton()
        button.setTitle("Button", for: UIControl.State.normal)
        button.setTitleColor(.white, for: .normal)
        button.backgroundColor = .systemGreen
        view.addSubview(button)
        
        button.translatesAutoresizingMaskIntoConstraints = false
        
        let safeArea = view.safeAreaLayoutGuide
        
        let leading = NSLayoutConstraint(item: button,
                                         attribute: .leading,
                                         relatedBy: .equal,
                                         toItem: safeArea,
                                         attribute: .leading,
                                         multiplier: 1,
                                         constant: 16)
        
        let trailing = NSLayoutConstraint(item: button,
                                          attribute: .trailing,
                                         relatedBy: .equal,
                                         toItem: safeArea,
                                          attribute: .trailing,
                                         multiplier: 1,
                                         constant: -16)

        let bottomSafeArea = NSLayoutConstraint(item: button,
                                                attribute: .bottom,
                                                relatedBy: .equal,
                                                toItem: safeArea,
                                                attribute: .bottom,
                                                multiplier: 1,
                                                constant: 0)
        bottomSafeArea.priority = .defaultHigh
        
        let bottomView = NSLayoutConstraint(item: button,
                                                attribute: .bottom,
                                                relatedBy: .lessThanOrEqual,
                                                toItem: view,
                                                attribute: .bottom,
                                                multiplier: 1,
                                                constant: -20) 
        NSLayoutConstraint.activate([leading,trailing,bottomView,bottomSafeArea])
        
    }
}
```
- 앵커레이아웃으로 불가능한것을 가능하게 하는 상황이 있음 그럴때 사용하면 유용하다.
- 논리적 오류가 발생했을때? 오류로 알려주지 않는다.
- 앱실행시 크래쉬가나서 앱이 죽어버린다.
- 앵커에서 오류체크가 가능한것은 제네릭기법을 사용하기때문이다
