# Today I Learned

# **학습내용**
## UIViewController
UIKit 앱의 뷰 계층 구조를 관리하는 객체

UIViewController 의 class는 모든 View Controller 의 공통적인 공유 동작을 정의한다.
클래스의 인스턴스를 직접 만드는 경우는 거의 없다.
대신 뷰 컨트롤러의 뷰 계층 구조를 관리하는 데 필요한 메서드와 속성을 하위 클래스로 만들고 추가한다.

### 뷰 컨트롤러의 주요 역할
- 데이터가 변경되면 View의 화면을 변경된 값으로 업데이트한다.
- View 를 통해 사용자와 상호작용에 응답한다.
- View 에 표시될 컴포넌트? 등의 크기, 전체 인터페이스 레이아웃 관리
- 앱내에서 다른 View Controller 와 다른 객체의 관계를 조정한다.

ViewController 는 관리하는 View 에 밀접하게 연결되어 있으며 View 계층 구조에서 이벤트 발생시 이벤트 처리에 참여합니다.
ViewController 는 UIResponder 객체이다.
ViewController 는 이벤트 처리에 관여해야 하므로 이벤트를 처리하거나, 부모 뷰 에 전달하는 옵션을 사용할수 있다.

ViewController 는 단독으로 사용되는 경우는 많지 않다.
대신 각각의 ViewController(전체 인터페이스 중의 일부) 를 소유하게 만들어 사용하는 경우가 많다.

예를 들면 ViewController, ViewController2 가있다면 ViewController2 는 앱내부의 화면중 한 화면을 담당하며 그 화면에서 일어나는 이벤트를 처리하거나 부모뷰에서 처리할수 있도록 하는 기능을 넣어줄수 있다.

### View 관리
- rootView
    - ViewController 는 View의 계층 구조를 관리하며 rootView 는 View 의 class 속성에 저장된다.
    - rootView 는 View stack 의 구조에서 최하위 라고 할수있다.
      이때 최상위 View 는 현재 앱에서 보여지는 화면을 의미한다.

- storyBoard 에서 ViewController 와 class 를 연결하여 사용 할수있다.
    - 그리하여 storyBoard 를 사용해 ViewController 를 조작하거나 코드를 이용하여 조작할수 있게된다.
    - 코드 화면에서 뷰컨트롤러 를 조작하려면 메서드를 이용해 뷰컨트롤러 를 로드할수 있다.`instantiateViewController(withIdentifier:)UIStoryboard`
        withIdentifier 는 storyBoard 에서 해당 뷰컨트롤러의 ID 를 뜻하며 동일한 ID를 메서드 파라미터에 작성해주면 된다.

- Nib 파일 을 사용하는 방법.
    - nib 파일을 사용하여 ViewController 에 연결된 View 만을 조작할수있다. 
    - 이때 다른 View 와 segue 또는 관계를 정의할수 없다.

## View Controller의 생명주기(Life-Cycle)
View Controller 에도 함수처럼 Life-Cycle 이 존재한다.
앱의 화면이 보여졌다 사라졌다 를 의미하는것 같다.

각각의 뷰는 각자 생명주기를 가지고 있다.

![](https://i.imgur.com/ASSlasn.jpg)

위의 그림은 View의 Life-Cycle 이다.
이때 Will은 미래, Did는 과거를 나타낸다.
하나씩 풀어보자면
- viewDidLoad(뷰가 로드 되기전)
- viewWillAppear(뷰가 나타나기 전)
- viewDidAppear(뷰가 나타난 후)
- viewWillDisappear(뷰가 사라지기 전)
- viewDidDisappear(뷰가 사라진 후)

### 1. viewDidLoad(뷰가 로드 되기전)
- "Called after the controller's view is loaded into memory"(뷰의 컨트롤러가 메모리에 로드되고 난 후에 호출된다.)
- 뷰의 로딩이 완료 되었을 때 시스템에 의해 자동 으로 호출되기 떄문에 리소스를 초기화 하거나 초기 화면을 구성하는 용도로 주로 사용된다.
- 화면이 처음 만들어질때 한 번만 실행됨
- 처음 한번만 초기화 되어야 하는 코드가 있을 경우 이곳에 작성한다.
### 2. viewWillAppear(뷰가 나타나기 전)
- 뷰가 화면에 나타나기 직전에 호출된다 볼 수 있다.
- viewDidLoad 와 viewWillAppear 는 둘다 화면이 나타나기 전에 호출되지만 차이점이있다.
- 차이점은 viewDidLoad 는 호출되고나서 다른 뷰로 갔다가 다시 돌아오는 상황에서는 호출되지 않는다.
- viewWillAppear 는 다른뷰로 갔다가 다시 돌아올때도 호출된다.
- 그러므로 modal 등 화면을 이동했다 다시 돌아왔을때 변경사항이 적용되게끔 할때에는 viewWillAppear 를 사용할수 있겠다.
### 3. viewDidAppear(뷰가 나타난 후)
- 뷰가 화면에 나타난 직후 실행된다.
- 뷰가 나타났다는 것을 컨트롤러에게 알리는 역할을 담당한다.
- 화면에 적용될 에니메이션을 담당할수 있다.
### 4. viewWillDisappear(뷰가 사라지기 전)
- 뷰가 사라지기 직전에 호출되는 함수
- 뷰가 삭제 되려고 하는것을 뷰 컨트롤러에 통지함.
### 5. viewDidDisappear(뷰가 사라진 후)
- 뷰 컨트롤러가 뷰가 제거되었음을 알려준다.

### 6. loadView
그렇다면 loadView 의 역활은 무엇일까??
loadView 는 컨트롤러가 관리하는 뷰를 만드는 역할을 한다.
loadView 가 뷰를 만들고 메모리에 올린후 viewDidLoad 가 호출된다 할수있다.

## View-Life-Cycle 예시
> **2개의 view에서 1view->2view 예시**
1view: viewDidLoad(뷰가 로드된 후)
1view: viewWillAppear(뷰가 나타나기 전!)
1view: viewDidAppear(뷰가 나타난후!)
1view: viewWillDisappear(뷰가 사라지기 전)
2view: viewDidLoad(2번째 뷰가 로드된 후)
2view: viewWillAppear(2번째 뷰가 나타나기 전)
1view: viewDidDisappear(1번째 뷰가 사라진 후)
2view: viewDidAppear(2번째 뷰가 나타난 후)

예시 처럼 1번view 에서 2번view 로 전환 될때 일어나는 상황이다.
view 가 2개 이상 있을 때는 
viewWillAppear 다음에 viewDidAppear 가 호출되는게 아니다!

> **2개의 view에서 1view->2view->1view 예시**
2view: viewWillDisappear(2번째 뷰가 사라지기 전)
1view: viewWillAppear(1번째 뷰가 나타나기 전)
2view: viewDidDisappear(2번째 뷰가 사라진 후)
1view: viewDidAppear(1번째 뷰가 나타난 후)

예시 처럼 다시 1번 화면으로 돌아왔을때 1view 의 viewDidLoad 는 호출되지 않는다.

viewDidLoad 는 현재 네비게이션 컨트롤러의 rootView 이기 때문에 한번만 호출되기 때문에 다시 화면전환을 해도 viewDidLoad 는 호출되지 않는다.

> **2개의 view에서 1view->2view->1view->2view 예시**
다시 2번view 로 이동하게되면 위의 설명과 다르게 이동할때마다 2view 의viewDidLoad 가 호출된다.

이유는 view 의 구조와 관계가 있다.
네비게이션 컨트롤러를 사용하게되면 stack 방식으로 저장되기 때문이다.

![](https://i.imgur.com/FfdsDfi.png)

여기서 stack 은 LIFO(Last-in First-out)구조 로 되어있다.
네비게이션 컨트롤러는 반드시 root View Controller 를 갖게됨으로
예시의 첫번째 View 가 root View 가된다.

view 는 stack 처럼 쌓이는 구조 이므로 1View 위에 2View 가 쌓이는(push) 방식이다. 당연히push 가 있으니 pop(스택 제거?) 도 존재한다.

다시 예시로 돌아가면 1View 에서 2View로 push 되고 2View에서 1View로 전환될때는 2View 가 pop(제거) 됨으로 다시 2View 로 push 될때는 새로운 2View 가 stack 에 쌓이게 되므로 매번 viewDidLoad가 호출되게 된다.

아래의 코드는 UIViewController의 view 관련 메서드 중 어느 메서드 내부에 위치하는 것이 좋을지 생각해봅시다
- 사용자 환영 애니메이션을 보여주는 코드(viewDidAppear)
- 뷰에 보여질 데이터를 불러올 코드(viewWillAppear)
- 배경음악을 재생할 코드(viewDidAppear)
- 배경음악을 중지할 코드(viewDidDisappear)(유튜브로 실험해봤는데 찰나의 순간이라 화면이 사라진후인지 전인지 모르겠다....)
- 노티피케이션 수신을 위한 옵저버 등록 코드(viewDidLoad)
- 노티피케이션 수신 중단을 위한 구독 중단 코드(viewWillDisappear)(요녀석도 애매하다..)
- 스토리보드로 구성한 뷰 요소의 초기값을 설정하는 코드(viewDidLoad)

참고
[애플 문서](https://developer.apple.com/documentation/uikit/uiviewcontroller)
[zeddios블로그](https://zeddios.tistory.com/43)
