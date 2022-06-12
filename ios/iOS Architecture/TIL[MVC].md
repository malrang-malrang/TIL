# Today I Learned.

## 학습내용
# MVC (Model-View-Controller)
>- M: Model
>- V: View
>- C: Controller
>
>MVC란 Model View Controller의 약자로 에플리케이션을 세가지의 역할로 구분한 개발 방법론이다.
>아래의 그림처럼 사용자가 Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자에게 전달하게 된다. 
>
>지금까지 사용하던 MVC 아키텍쳐 패턴, 가장 유명하며 자연스럽게 사용되는 아키텍쳐다.
>
>처음 MVC 를 공부해서 적용했을때 기존의 코드와 비교했을때 코드가 훨씬 정리가 되어있는 느낌을받아 좋아했던게 기억난다..
>
>## Traditional MVC
>![](https://i.imgur.com/139JpMR.png)
>
>위의 다이어그램을 보면 Model, View, Controller 가 서로 강하게 연결되어있는 모습을 볼수 있다.
>
>View는 사용자의 액션을 Controller 에게 전달하고 Controller 는 이에 따른 데이터의 갱신을 Model 에 요청한다.
>Model에서 데이터의 갱신이 일어나고 상태변화를 View에게 전달한다.
>View는 Model 에서 갱신된 데이터에 맞춰 갱신하게된다.
>
>이렇게 강하게 연결된 셋은 독립성이 낮기 때문에 이들 각각의 재사용성이 떨어지며 그렇기 때문에 IOS 개발에서는 전통적인 MVC 아키텍쳐 말고 다른 MVC 아키텍쳐를 제시하고 사용한다.
>
>## Apple's MVC
>![](https://i.imgur.com/gvaoS46.png)
>애플이 제시한 Cocoa MVC 에서 Controller 는 View와 Model 의 중재자로 View와 Model의 직접적인 연결을 막는 구조 잇다.
>
>이는 전통적인 MVC 보다 높은 독립성의 보장을 기대한다.
>
>하지만 실제 개발에서 Cocoa MVC 아키텍쳐에서 Controller 의 역할은 UIViewController 가 담당하게 된다.
>
>ViewController 는 View 를 소유하게되고 View들의 LifeCycle과 강하게 연결되게 된다.
>
>그렇기 때문에 View와 Controller의 분리가 쉽지 않으며 Controller와 연관되어있는 View의 재사용이 어려워지게된다.
>
>이렇게 View와 Controller가 강하게 연결되면 테스트하기 어려워지며 View에서 사용될 사용자의 액션과 이에따른 메서드뿐만 아니라 UIViewController 에서 일어나는 행위들로(네트워크통신, Delegate 등등) Controller 는 방대해지고 이를 흔히 Massive ViewController 라고 부르기도 한다.
>
>아래의 그림처럼 강하게 연결되어있는 모양이 된다.
>![](https://i.imgur.com/X2atKi7.png)
>
>View의 생성과 배치에 관련된 코드들이 Controller안에 위치하게 되고 이들을 갱신하는 코드도 Controller 내부에 위치하게 된다.
>
>View를 테스트하기 위해서는 Controller의 View LifeCycle 관련 메서드(ViewDidLoad, ViewWillAppear 등등)의 호출이 없다면 진행할 수 없기 때문에 View와 Controller 가 강하게 연결되어 있다는 것을 알수 있다.
>
>위에서 언급했던 좋은 아키텍쳐의 기준에 얼마나 부합하는지 확인해보자.
>- **Distribution :**
>   -  View와 Model은 분리되어 있지만 View와 Controller는 강하게 연결되어 있다.
>- **Testability :**
>    -  View와 Controller가 강하게 연결되어 있기 때문에 오로지 Model만 테스팅을 진행할 수 있다.
>- **Easy of Use :** 
>   - 여러 아키텍쳐 중 가장 적은 코드를 필요로 하며 가장 친숙한 아키텍쳐 패턴으로 많은 경험이 없는 개발자들도 쉽게 유지 보수할 수 있다.
>
>개발 속도에 있어서는 가장 빠른 아키텍쳐 패턴이라 할수 있겠다.
>하지만 아주작은 프로젝트라 하더라도 많은 유지보수 비용이 들어가게 된다.
