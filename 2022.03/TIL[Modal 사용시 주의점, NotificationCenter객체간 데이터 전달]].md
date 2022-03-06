# Today I Learned

###궁금증
노티피케이션을 사용할때 옵저버를 해제하지 않고 해당 클래스가 종료되면(view가 pop, dissmis를 의미) 옵저버도 같이 해제되는걸까?
- 1view, 2view 에서 Notification center 을 사용할때 2view 에서 옵저버를 해지 하지않을경우 2view가 메모리에서 제대로 해지되지않고 좀비처럼 남아있을수 있다.
    - (자동으로 같이 메모리에서 해제될줄 알았다..)
1view, 2view 에서 1view->2view로 modal present 2view->1view 로 dissmis 하게될경우 1view 의 라이프사이클은 어떻게 되는걸까?
- 네비게이션과 동일하게 1view 는 viewDidLoad 는 처음 한번만 호출되며 그후 dissmis 로 돌아오게 될 경우 1view 의 viewWillAppear 가 호출된다.
    - (네비게이션 은 stack 방식으로 저장되기때문에 1view 는 메모리 해제가 되지않아 pop 되어도 viewWillAppear 가 호출 되는것을 알고 있었지만 Modal 의경우 화면이 대체? 되는걸로 인식 했기 때문에 dissmis 하면 1view 는 다시 viewDidLoad 부터 시작할것으로 생각했었다.)

## **학습내용**
### NotificationCenter 를 이용해 객체 간의 데이터 전달

NotificationCenter 를 이용해 1view에서 2view 로 데이터를 전달하는 방법은 아직 모르겠다.
1view 에서 2view 로 이벤트 발생으로 인해 데이터를 전달 해줄때 2view 는 현재 메모리에 올라와있지 않기 때문인것 같다. 이벤트가 발생했으나 2view 가 아직 없는 상황???

2view 에서 1view 로 데이터를 전송하는것은 간단! 
1. 1view 에서 addObserver 를 등록해주고 이벤트를 전달 받으면 어떤 기능을 실행 할 것인지 까지 설정해준다.
2. 2view 에서 이벤트를 전송해줄 post 를 등록!(액션? 에다가 등록)
3. 2view 에서 post 를 전송해주는 Action 에서 userInfo 를 이용해 값을 전달 해준다.
4. 1view 에서 이벤트 알림을 받고 처음 설정해두었던 기능을 실행!
5. 호출된 메서드에서 파라미터를 Notification 타입으로 받게되면 2view 에서 전달해준 데이터를 사용할수있다! 
6. 이때 전달받은 데이터는 key, value 값으로 되어있는 튜플타입! 딕셔너리! 얘내 다운캐스팅,바인딩해서 가공해서 쓰면된다!

## 알아낸것들
### Modal 사용시 주의점!
view가 가로,세로 모드 일 경우 Modal을 present 할때 화면 설정을 fullscreen,sheet 등 설정해주지 않아도 fullscreen 처럼 전체 화면을 가리게 된다.
하지만 이때 설정이 fullscreen이 아니라 auto, sheet등 다른것이라면 해당 뷰에서 viewWillDisappear(뷰가 사라지기 전),viewDidDisappear(뷰가 사라진 후) 얘내가 호출되지 않게된다. 주의하도록 하자...

### Dictionary 다운캐스팅
Notification center 를 이용해 userinfo 로 데이터를 전송할때 userinfo 에 저장될 타입은
key, value 로 들어오는 튜플타입이다. 딕셔너리와 같다?

이때 key으로 넣어준 값은 AnyHashable 타입으로 업캐스팅되며 value값은 Any업캐스팅 된다.
userinfo를 전달받은곳 에서 데이터를 활용해주기 위해서는 다운 캐스팅을 이용해 userinfo 의 key,value 의 타입을 바꿔 주어야한다. 
`guard let userInfo = notification.userInfo as? [Int : String] else { return }`
와 같이 key, value 를 같이 다운캐스팅 해줄수있다.
이거 몰라서... 삽질 엄청했다 까먹지말자..value 들 일일히 다운캐스팅 하고 난리도 아니었다...
