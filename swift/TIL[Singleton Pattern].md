# Today I Learned

# **학습내용**
## Singleton Pattern

Singleton Pattern 이란 특정 용도로 사용될 객체 '한개만' 만들어서 여러곳에서 공용으로 사용할때 사용하는 디자인 패턴 이다.
(전역 변수 같은느낌?)

사용 예시
```swift
class Malrang {
   static let malrang = Malrang()
    
    var name = String?
    var age = Int?
    private init() {}
}
```
위의 예시처럼 더이상 Malrang 의 객체를 만들수 없게끔 정의하여 사용할수 있다.
Malrang 타입의 객체는 새로 초기화,생성 할수없 도록 init에 접근제한을 걸어준뒤 내부에서 Malrang의 타입을 갖는 객체를 어디서든 접근하여 사용할수 있도록 static 키워드를 이용해 어디서든 malrang 이라는 프로퍼티를 사용할수 있도록 한다.

이때 static 을 이용해 타입 프로퍼티로 인스턴스를 생성하게되면
lazy 하게 초기화 되기때문에 사용될때 초기화 된다고 할수있다.

### Singleton Class 접근 방법 

위에서 생성했던 static 프로퍼티를 이용해 접근한다.
타입명.타입프로퍼티네임 방식으로 접근!

여러개의 객체에서 Singleton 에 접근하여 사용하는 예씨
```swift
let doogie = Malrang.malrang
doogie.name = "doogie"
```
```swift
let mmim = Malrang.malrang
mmim.age = 25
```
예시 에서 doogie, mmim 은 하나의 인스턴스를 공유중이다.

### Singleton의 장단점
#### 장점
- Singleton Instance 는 전역에서 사용할수 있기에 다른 객체들과 자원 공유가 쉽다.
- 한개의 instance 만 만들어 사용하기 때문에 메모리 낭비를 방지 할수 있다.

#### 단점
- Singleton Instance 가 많은 일을 할 경우(다른 객체랑 많은 데이터를 공유 할경우) 객체 끼리의 결합도가 높아져 유지보수 하기 어렵다.(객체 지향 설계원칙 어긋남)
(기능이 몇개 안들어있는 객체와 비교하자면 기능 몇개 안들어간놈은 다른곳에서 재사용하기 쉽지만 Singleton Instance 에 기능이 대략10개인 경우 얘가 가지고 있는 기능중 2개만 필요한데 얘를 가져다 쓰기엔 안쓰는 기능이 8 가지나 되서 메모리적으로도 비효율적..그래서 재사용해 가져다 쓰기 어려움.)

### Singleton 사용예시
 >UIApplication.shared
 UserDefaults.standard
 UIApplication.shared
 FileManager.default
 NotificationCenter.default
 DispatchQueue.main
 
위의 예시는 Singleton Pattern 의 사용예시 에서 주로 나오는 애들이다.

하지만 이예시중 NotificationCente 같은경우 새로 NotificationCente 타입을 만들어 정의하여 사용할수도 있으며
NotificationCente.Default 를 사용해 따로 정의하지 않고 사용도 가능하다. 

Singleton Pattern 이라 함은 하나의 인스턴스만 만들수 있어야하며 인스턴스를 다른 객체와 공유하여야하는데 같은 타입의 인스턴스를 더 만들수 있다는 부분으로 봐서 Singleton Pattern 이라 할수는 없을것 같다. 
Singleton Pattern처럼 사용할수도 있지만 애플이 init을 private로 막아둔게 아니라 커스텀하여 사용할수 있도록 오픈 해둔것 같다.
