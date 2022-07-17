# Today I Learned.

## 학습내용
# Coordinator 패턴
iOS에서는 화면 전환을 담당하는 컨트롤러인 UINavigationController가 있다.

Stack방식으로 새로운 화면을 push혹은 present하고, 이전 화면으로 돌아가기 위해 pop혹은 dissmis 한다.
가장 첫 화면을 기준으로 새로운 화면으로 넘어갈때 마다 하나씩 쌓이게 되고 뒤로가기 버튼을 통해 이전에 방문했던 화면들을 순서대로 꺼낼게된다.

```swift
navigationController?.pushviewController(nextViewController, animated:true)
```
요렇게 제공되는 메서드를 사용해 간단하게 화면을 전환할수 있지만 앱이 커지고 화면이 많아진다면 사용하기 버거워진다.
예를 들어 화면을 전환하는 메서드가 모든 뷰마다 존재한다던지 하는 중복코드가 발생할수있으며 화면을 전환하는 코드가 화면을 전환하려는 viewController를 의존하게된다.

요런 문제를 해결하기 위해 만들어진것이 Coordinator 패턴이다!

## Coordinator란?
화면의 흐름을 제어해주는 역할 을 하는친구다!

![](https://i.imgur.com/Wx2rtvS.png)

요런 느낌이라고 생각하면된다!

기존에는 StoryBoard 혹은 ViewController에서 화면전환(라우팅)을 해줬는데 Coordinator를 사용하면 Coordinator 요녀석이 VC의 이벤트도 담당하고 화면에대한 동작을 처리하게된다!

VC의 책임이 줄어들겠죠???

요녀석을 사용하는 이유는 화면간 연결을 더 쉽게 관리할수 있으며 VC를 더 쉽게 재사용하게한다!

## Coordinator Pattern 사용해보기
Coordinator Pattern을 사용하기 위해서는 앱의 시나리오에 따라 조금 다르겠지만 보통 아래 작성된 2개를 프로젝트에 적용시킨다.
1. AppCoordinator가 존재한다.
2. 모든 Coordinator 에는 하위 Coordinator가 있다.

지금 하고있는 프로젝트에 적용시킬것이기 때문에 구조는 요렇게 될것같다!


그럼이제 만들어보자!

![](https://i.imgur.com/O4Z4wns.png)

지금부터 만들 Coordinator 의 구조는 요렇게!

AppCoordinator가 존재하고 하위에 ListViewCoordinator, DetailViewCoordinator가 존재한다!

App을 처음 실행하게되면 AppCoordinator가 어떤 화면을 보여줄것인지 담당해줄거고 View끼리 화면전환을 할때에도 AppCoordinator를 통해 화면전환이 이루어지게 된다!

그럼 차근차근 한개씩 만들어보자!

### 1. Coordiantor Protocol을 만든다.
```swift
protocol Coordinator: AnyObject {
    var childCoordinators : [Coordinator] { get set }
    func start()
}
```
Coordinator 프로토콜은 필수로 자식 Coordinator와 화면을 보여줄 start() 메서드를 요구한다.

### 2. Coordinator프로토콜을 채택하는 AppCoordinator를 만든다.
```swift
final class AppCoordinator: Coordinator {
    var childCoordinators: [Coordinator] = []
    private var navigationController: UINavigationController!
        
    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }
    
    func start() {
         self.showListView()
    }
    
    private func showListView() {
        let coordinator = TodoListViewController(navigationController: self.navigationController)
        coordinator.start()
        self.childCoordinators.append(coordinator)
    }
    
    private func showDetailView() {
        
    }
}
```
Coordinator프로토콜을 채택했으니 프로토콜에 명시된 childCoordinators와 start()메소드를 구현해준다.

navigationController는 화면을 전환할때 push혹은 present하기위해 사용된다.

### 4. SceneDelegate설정
```swift
final class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?

    func scene(
        _ scene: UIScene,
        willConnectTo session: UISceneSession,
        options connectionOptions: UIScene.ConnectionOptions
    ) {
        guard let windowScene = (scene as? UIWindowScene) else {
            return
        }
        
        let window = UIWindow(windowScene: windowScene)
        self.window = window
        
        let navigationController = UINavigationController()
        self.window?.rootViewController = navigationController
        
        let coordinator = AppCoordinator(navigationController: navigationController)
        coordinator.start()

        self.window?.makeKeyAndVisible()
    }
}
```
앱을 처음 실행했을때 어떤 화면을 보여줄것인지 AppCoordinator를 사용해 정의해주면 된다!

window와 UINavigationController를 만든다.

그리고 AppCoordinator인스턴스를 만들어 주고 이니셜라이저 인자값으로 만들어둔 UINavigationController를 넘겨준뒤 start메소드를 호출한다.
start()메서드는 앱이 실행될때 처음 보여질 화면이다.

### 5–1. ListViewCoordinator구현
```swift
final class ListViewCoordinator: Coordinator {
    var childCoordinators: [Coordinator] = []

    var navigationController: UINavigationController

    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }

    func start() {
        let viewController = TodoListViewController()
        self.navigationController.pushViewController(viewController, animated: true)
    }
}
```
요녀석은 ListViewController화면을 보여주게할 Coordinator인데 start()메서드 내부에 TodoListViewController인스턴스를 만들어주고 있다.

그후 AppCoordinator에게서 받은 navigationController를 사용해 push해준다.

요렇게 하면 앱을 실행하면 ListView가 화면에 보여지게된다.

### 5–2. TodoListViewController구현.
아래는 기존 프로젝트 코드
```swift
 private func bind() {
        self.navigationItem.rightBarButtonItem?.rx.tap.asObservable()
            .subscribe(onNext: { [weak self] in
                self?.presentDetailView() })
            .disposed(by: disposeBag)
    }
    
    private func presentDetailView() {
        let viewController = DetailViewController()

        let navigationController = UINavigationController.init(rootViewController: viewController)
        navigationController.modalPresentationStyle = .formSheet
        self.present(navigationController, animated: true)
    }
```
네비게이션바의 오른쪽 아이템 탭하면 presentDatilView() 메서드를 실행하게 되어있다.
이전까지는 이렇게 화면전환시켜줬다!

### 5–3. TodoListViewControllerDelegate구현
TodoListViewcontroller가 navigationbaritem탭하면 TodoListViewCoordinator에게 전달해주어야 하기때문에 TodoListViewControllerDelegate를 구현해주어야 한다.

```swift
protocol TodoListViewControllerDelegate {
    func showDetailView()
}
```
요렇게!

ListViewController가 ListViewCoordinator한테 화면전환 시켜줘!! 하는거랑 같다!

그럼 이제 TodoListViewCoordinator가 요녀석을 채택해주면된다!
```swift
final class TotoListViewCoordinator: Coordinator, TodoListViewControllerDelegate {
    var childCoordinators: [Coordinator] = []
    var delegate: TodoListViewControllerDelegate?

    var navigationController: UINavigationController

    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }

    func start() {
        let viewController = TodoListViewController()
        viewController.delegate = self
 
        self.navigationController.pushViewController(viewController, animated: true)
    }

    func showDetailView() {
        self.delegate?.showDetailView()
    }
}
```
요렇게!

채택했으니 TodoListViewControllerDelegate에 명시된 showDetailView()메서드와 delegate프로퍼티를 만들어줍니다!
delegate프로퍼티는 상위 Coordinator에서 전달받을건데 TodoListViewCoordinator가 상위 AppCoordinator에게 화면 전환 시켜달라고 요청하기위해 필요하다!

### 5-4 TodoListViewController delegate 구현
```swift
 var delegate: TodoListViewCoordinator?

 private func bind() {
        self.navigationItem.rightBarButtonItem?.rx.tap.asObservable()
            .subscribe(onNext: { [weak self] in
                self?.delegate?.showDetailView() })
            .disposed(by: disposeBag)
    }
```
delegate프로퍼티를 하나 만들어줬는데 저놈은 TodoListViewControllerDelegate 타입이니까 showDetailView() 메서드를 가지고 있을걸 보장한다!

그럼 bind()메서드 내부에서 저녀석을 사용하면되겠군!
self?.delegate?.showDetailView() 를 호출!

하지만 아직 화면전환은 안된다.

왜?TodoListViewCoordinator는 또 AppCoordinator에서 주입받은 delegate를 이용해야 하는데 AppCoordinator에서 주입해주지 않았기 때문이다.

TodoViewController: 나 탭눌렸어! TodoListViewCoordinator화면전환해줘! 
TodoListViewCoordinator: 알겠어! AppCoordinator한테 해달라고할게! 
요런상황인데 TodoListViewCoordinator는 AppCoordinator를 모르는 상황이다.

### 5-5 AppCoordinator 수정
```swift
final class AppCoordinator: Coordinator, TodoListViewControllerDelegate {
    
    var childCoordinators: [Coordinator] = []
    var navigationController: UINavigationController
        
    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }
    
    func start() {
        self.showListView()
    }
    
    func showListView() {
        let coordinator = TotoListViewCoordinator(navigationController: self.navigationController)
        coordinator.delegate = self
        coordinator.start()
        self.childCoordinators.append(coordinator)
    }
}
```
우선 AppCoordinator에 TodoListViewControllerDelegate를 채택시켜준다!

그래야 TodoListViewCoordinator의 delegate에 AppCoordinator 자기자신을 할당해줄수있게된다.

위에서 만들어둔 AppCoordinator의 showListView() 메서드를 보자!

TodoListViewCoordinator를 초기화하고 나서 내부의 프로퍼티인 delegate에 자신을 할당해주면된다!

그럼 초기화된 TodoListViewCoordinator의 delegate는 AppCoordinator니까 아까위의 문제를 해결할수있게된다!

그럼이제 TodoListViewCoordinator의 필수메서드 showDetailView() 차례다. 요녀석은 이제 맨처음 화면 을 보여줄때와 똑같다.

TodoListViewController로 화면전환을 한때에는 TodoListViewCoordinator를 구현해서 시켜줬다!

DetailView로 화면 전환하는것도 똑같다! DetailViewController로 화면전환을 시켜주기위해 DetailViewCoordinator라는 AppCoordinator의 하위 코디네이터를 만들어준뒤 그놈한테 화면 보여주라고 시키면된다!

### 6 DetailViewCoordinator와 DetailViewControllerDelegate

```swift
protocol DetailViewControllerDelegate {
    func start()
}

final class DetailViewCoordinator: Coordinator, DetailViewControllerDelegate {
    var childCoordinators: [Coordinator] = []
    var delegate: DetailViewControllerDelegate?
    weak var detailViewController: DetailViewController?

    var navigationController: UINavigationController

    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }

    func start() {
        let viewController = DetailViewController()
        viewController.delegate = self
        detailViewController = viewController

        let navigationController = UINavigationController(rootViewController: viewController)
        navigationController.modalPresentationStyle = .formSheet
        self.navigationController.present(navigationController, animated: true)
    }
}
```
이렇게 지끔까지만들어둔걸 똑같이 만들어주면된다!

흐름을 정리해보자 
1. TodoListViewController: 버튼눌렸어 화면전환해줘!
2. TodoListViewCoordinator: 알겠어! AppCoordinator한테 부탁할게!
3. AppCoordinator: DetailViewController로 화면전환시켜줘!DetailViewCoordinator!
4. DetailViewCoordinator: 알겠어 DetailViewController화면 띄울게!

요렇게 된다!

## 참고한 문서및 자료
[zeddios](https://zeddios.medium.com/coordinator-pattern-bf4a1bc46930)
[Ian's](https://duwjdtn11.tistory.com/644)
[레나참나](https://lena-chamna.netlify.app/post/ios_design_pattern_coordinator_advanced/)
