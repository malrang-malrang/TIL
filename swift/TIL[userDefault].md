# Today I Learned

# **학습내용**
## UserDefaults
설정해둔 데이터가 저장되어있다면 앱을 껏다 켜도 데이터가 사라지지 않고 남아있어 데이터를 활용할수있다.
- 간단히 말하면 데이터 저장소 라고 할수있다.
- 전역에 위치하여 어디서든 데이터를 쉽게 읽고 저장할수있다!
- 객체를 저장할수도 있다.
- UserDefaults는 사용자 기본 설정과 같은 단일 데이터 값에 적합하다.
- 대량의 유사한 데이터 를 저장해야하는 경우에는 sqlite 데이터베이스가 더 적합하다.

## UserDefaults 사용법
- UserDefaults 는 [key: Value] 로 데이터를 저장하는데 이때 key 값은 String 이다.
- UserDefaults 를 이용해 값을 저장할때는 set메서드를 이용한다.
- UserDefaults 에 값을 저장할때 기존의 key 와 같은 key를 사용할경우 value 가 덮어씌워지게 된다.
- UserDefaults 의 값을 가져올때는 키값인 String 값을 입력하여 꺼내어올수 있다.
- UserDefaults 의 값을 삭제할때는 removeObject 메서드를 이용한다.

```swift
class ViewController: UIViewController {

    @IBOutlet weak var 레이블: UILabel!
    
    @IBAction func 레이블값변경(_ sender: UIButton) {
        let malrang = "malrang"
        
        레이블.text = malrang
        
        UserDefaults.standard.set(malrang, forKey: "malrangName")
    }
    
    
    @IBAction func userDefault삭제(_ sender: UIButton) {
        UserDefaults.standard.removeObject(forKey: "malrangName")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        레이블.text = UserDefaults.standard.string(forKey: "malrangName")
    }
}
```
위의 예시로 설명하면 버튼을 클릭 하게 되면 화면의 레이블 값이 변경되는 코드이다. 

화면의 레이블 값이 변경되고 변경된 malrang 의 값이 UserDefaults 에 저장되게 된다.

그후 앱을 종료하고 다시 실행하게되면 버튼을 누르지 않아도 레이블값이 변경된 채로 실행되게 된다.

그후 삭제 버튼을 누르면 userDefault 에 저장된 값이 삭제되고 앱을 다시 실행하게되면 레이블이 변경전으로 돌아온걸 알수있다.

## 실행 화면

![](https://i.imgur.com/wnjX7Nm.gif./pic/pic1s.png =450x)
