# Today I Learned

# **학습내용**
**KVO**
- 객체간 소통 하기!
- 한객체가 다른 객체와 소통은 해야하지만 병합되기는 싫을때 사용한다.
- 혼자 끙끙댔던 로또 번호 추출기앱 에서 사용하면 좋을것같다.

View와 ViewController 간, 또는 각각의 객체 사이에 소통이 필요할 때 사용한다. 어떤 이벤트가 A에서 일어나면 B에 알려주어 적절한 조치를 취한다.

## KVO(MVC 에서 유용한듯 하다)
- 모델 객체의 상태를 뷰 및 컨트롤러 레이어의 객체와 동기화하는 데 사용할 수 있다.
- NSObject 를 상속 받아야 사용할수있다!
- NSObject를 상속 받기위해서는 import Foundation 을 사용해야함.
- NSObject를 상속 받아야 하기때문에 class 에서 사용가능!
- KVO를 사용하여 일대일, 일대다 관계를 형성하여 다른 객체의 모든 속성을 관찰할수있다.
- 관찰중인 속성이 변경되면 변경된값과 이전값이 무엇인지 알아낼 수 있다.
- 관찰자는 변경유형에 대해 정보를 받을뿐 아니라 어떤 객체가 관련되어 있는지 알수있다.

![스크린샷 2022-02-23 오후 10 06 31](https://user-images.githubusercontent.com/88717147/155325456-dc4ef3a5-372f-44bb-b87c-c17811c9f0a5.png)

### KVO 와 NSNotification, NSNotificationCenter 의 차이점
KVO 와 NSNotification, NSNotificationCenter 는 메커니즘이 비슷하지만 차이점이있다.
- KVO 는 A 객체에서 B 객체의 프로퍼티가 변화됨을 감지할 수 있는 패턴이다.
- NSNotification 및 NSNotificationCenter 는 관찰자로 등록된 모든 객체에게 알림을 보낸다.

NSNotification, NSNotificationCenter은 Controller와 다른 객체 사이의 관계를 다룬다면 KVO 패턴은 객체와 객체 사이의 관계를 다루는데 적합할것 같다.
메소드나 다른 액션에서 나타나는 것이 아니라 프로퍼티의 상태에 반응하는 형태이다.

### Implementing KVO
NSObject 를 상속받으면 재정의할 필요가 거의 없이 KVO 기본 구현을 제공한다.
따라서 모든 Cocoa 객체는 NSObject 를 상속받아 KVO 사용 이 가능합니다.
KVO 알림을 받으려면 다음을 수행해야 합니다.

- 관찰된 클래스가 관찰하려는 속성에 대해 준수하는 Key-Value 관찰인지 확인해야 합니다.
    - KVO 준수를 위해서는 관찰된 객체의 클래스도 KVC를 준수해야 하며 속성에 대한 자동 관찰자 알림을 허용하거나 속성에 대한 수동 키-값 관찰을 구현해야 합니다.
- 값이 변경될 수 있는 객체에 관찰자를 추가합니다. 
    - addObserver:forKeyPath:options:context:를 호출하여 이 작업을 수행합니다.
    - 관찰자는 또 다른 객체 라고 생각 하는게 편한것 같다.
- 옵저버 객체(변경된 값을 받을 객체)에서 observeValueForKeyPath:ofObject:change:context: 메소드를 구현합니다. 이 메서드는 관찰된 객체의 속성 값이 변경될 때 호출됩니다.

### KVO Is an Integral Part of Bindings (OS X)
Cocoa 바인딩(연결)은 많은 "접착 코드(인스턴스를 생성해서 상호간 연결하는 등?)" 를 작성할 필요 없이 Model, View 를 동기화(값이 변경되면 다른곳에서도 똑같이 변경됨) 시켜주는 OS X 기술입니다. Interface Builder 인스펙터를 통해 View의 속성과 데이터 사이에 매개 연결을 설정하여 하나의 변경 사항이 다른 하나에 반영되도록 "바인딩(연결?)"할 수 있습니다.
