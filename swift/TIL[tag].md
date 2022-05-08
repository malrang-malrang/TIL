# Today I Learned

# **학습내용**
## tag 사용법
- 코드로 태그를 부여하는것도 가능하며 storyBoard에서도 가능하다.

![](https://i.imgur.com/Ec6GCJV.png)
위와 같이 label 에 tag 를 달아주는것이 가능!

![](https://i.imgur.com/ON5TxuN.png)

아래 쪽에 Tag 라고 하는 공간이있으며 기본값은 0 이기때문에 0으로 설정되어있는 모습.

- tag 를 사용하여 label,button 등 UI컴포넌트에 enum 의 원시값 처럼 Int 값을 넣어 사용할수있다.
- tag에 들어올수있는 값은 Int(1,2,3...)만 가능하다.

## 활용예시 
- 각각의 버튼을 하나의 @IBAction 메서드에 연결하여 sender 를 이용하여 활용한 모습이다.

```swift
@IBAction func touchToOrderJuice(_ sender: UIButton) {
            switch sender {
            case ddalBaJuiceOrderButton:
                makeJuice(menu: .ddalBaJuice)
            case mangKiJuiceOrderButton:
                makeJuice(menu: .mangKiJuice)
            case strawberryJuiceOrderButton:
                makeJuice(menu: .strawberryJuice)
            case bananaJuiceOrderButton:
                makeJuice(menu: .bananaJuice)
            case pineappleJuiceOrderButton:
                makeJuice(menu: .pineappleJuice)
            case kiwiJuiceOrderButton:
                makeJuice(menu: .kiwiJuice)
            case mangoJuiceOrderButton:
                makeJuice(menu: .mangoJuice)
            default:
                presentBasicAlert(title: "경고", message: "알 수 없는 오류.")
            }
            showStock()
       }
```
sender 에따라 모든 케이스를 분기로 나누어 실행하여야 했기때문에 switch를 사용했었다.
이때 tag를 활용하면 좀더 간결하게 작성이 가능해진다.

```swift
@IBAction func touchToOrderJuice(_ sender: UIButton) {
        guard let menu = Menu(rawValue: sender.tag) else {
            presentBasicAlert(title: "경고", message: "알 수 없는 오류.")
            return
        }
        makeJuice(menu: menu)
        showStock()
    }
```

tag를 활용하기위해 Menu열거형의 case 들에게 각각 원시값(1, 2...)을 넣어주었고 sender의 tag번호와 menu의 원시값을 매칭하여 makeJuice 메서드를 실행하게하는 로직이다.

- 원리는 Button 을 사용자가 입력하게되면 어떤 버튼을 눌렀는지 sender로 전해지게 된다.
- 이때 sender는 tag값을 넣어주었으니 1,2,3 등 처럼 사용할수 있게된다. 
- sender.tag의 값을 Menu의 원시값으로 넣어주어 Menu의 sender.tag와 같은 원시값을 가지는 case가 선택되게 된다.

### 개인 생각
코드가 많이 간결해지긴 했지만 전의 방법과 비교했을때 가독성이 떨어진다? 라는느낌은 있었다. 앞으로도 활용하여 사용할수있는 상황이 생기면 가독성 부분에서 충분히 고민해보고 사용하도록 하자!
