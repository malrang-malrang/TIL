# Today I Learned.

## 학습내용
# IOS Layout Cycle, Drawing Cycle
>Application에서 UI화면에 어떤것을 보여줄것인지 어떻게 업데이트 되는지 이해하기 위해서는 MainRunLoop, Update cycle 에대해 이해하고 있어야한다.
>
>![](https://i.imgur.com/oBxhr3L.png)
>## MainRunLoop
>어플리케이션이 실행되면 iOS의 UIApplication이 매인 스레드에서 main run loop를 실행시킨다.
> main run loop는 돌아가면서 터치 이벤트, 위치의 변화, 디바이스의 회전 등의 각종 이벤트들을 처리하게 된다.
>  이러한 처리 과정은 각 이벤트들에 알맞는 핸들러를 찾아 그들에게 권한을 처리 권한을 위임하며 진행된다.
>
>이렇게 발생한 이벤트들을 모두 처리하고 권한이 다시 main run loop로 돌아오게 되고 이 시점을 update cycle이라고 한다.
>
> ![](https://i.imgur.com/sE9mVIe.png)
> ## Update cycle
> main run loop에서 이벤트가 처리되는 과정에서 버튼을 누르면 크기나 위치가 이동하는 애니메이션과 같이 layout이나 position 값을 바꾸는 핸들러가 실행될 때도 있다.
>이러한 변화는 즉각적으로 반영되는 것이 아니다.
> 
>시스템은 이러한 layout이나 position이 변화되는 View를 체크한다.
>그리고 모든 핸들러가 종료되고 main run loop로 권한이 다시 돌아오는 시점인 update cycle에서 이런 View들의 값을 바꿔주어 position이나 layout의 변화를 적용시킨다.
>
>즉 postion이나 layout 값을 변경하는 코드와 실제로 변경된 값이 반영되는 시점에는 시간차가 존재한다는 뜻이다.
>
>자 그럼 이제 LayoutCycle 을 보자..!
>
>---
># LayoutCycle
>앱의 컨텐츠들이 화면에 어떻게 보여지는지를 나타내는 과정이다.
>
>레이아웃 사이클은 세가지 단계로 이루어진다.
>
>**1. 제약조건(Constraints)**
>- 오토레이아웃의 제약 조건을 업데이트한다.
>- 제약 조건은 뷰를 실제로 배치하는데에 영향을 주지 않는다.
>- 제약 조건의 갱신은 뷰 계층구조에서 하위뷰에서 상위뷰의 순서로 이루어진다.
>
>**2. 레이아웃(Layout)**
>- 제약조건의 값을 이용해 View가 위치해야할 수치값을 업데이트한다.
>- 레이아웃은 구체적인 뷰의 frame값 이다.
>- 해당 단계에서 뷰의 Center와 Bounds를 결정한다.
>- 레이아웃의 갱신은 뷰계층구조에서 상위뷰에서 하위뷰의 순서로 이루어진다.
>- 제약조건을 가지고 화면에 보여주기위해(그리기위해) 각 컨텐츠들을 배치하는 단계.
>
>**3. 그리기(Draw)**
>- 레이아웃 단계에서 구한 frame을 CoreGraphics를 사용하여 화면에 그린다. UIView의 draw(_:) 메서드가 이에 해당한다.
>
>아래의 이미지는 ViewController와 ViewController의 View의 각 메서드 호출 순서다.
>(ViewLifeCycle, LayoutCycle)
>아래의 사진 순서로 ViewController와 ViewController의 View의 각 메서드가 호출된다.
> 
>![](https://i.imgur.com/Y3yn34v.png)
>
>---
># UIView 
>## 1. 제약 조건(Constraints)
>### updateConstraints
>![](https://i.imgur.com/m58skgz.png)
>
>View의 제약조건을 갱신한다.
>해당 메서드를 재정의 하면 제약 조건의 변경사항을 최적화 할 수 있다.
>효율적인 앱을 만들기위해서는 변경이 필요한 항목만 변경해야 한다.
>
>View에 영향을 미치는 변화가 발생한 후 즉시 제약조건을 갱신하는 것이 일반적으로 더 깔끔하고 쉽다.
>예를들면 버튼을 눌러 제약조건을 변경하려면 버튼을 눌렀을때 호출되는 함수에서 직접 변경하는게 낫다.
>제약조건을 변경하는것이 느리거나 여러변의 쓸모없는 변화가 발생하는 경우에만 해당 함수를 재정의 해야한다.
>
>```
>제약 조건의 갱신은 뷰 계층구조에서 하위뷰에서 상위뷰의 순서로 이루어지기 때문에
> 상위뷰의 updateConstraints 메서드는 가장 마지막에 호출해야 한다.
>```
>이 함수는 직접 호출하면 안된다.
>시스템이 적당한 타이밍에 호출해준다.
>시스템에 제약 조건의 갱신을 요청하기 위해서는 `setNeedsUpdateConstraints` 메서드를 호출해야한다.
>
>setNeedsUpdateConstraints 메서드를 호출하면 View의 제약조건을 갱신해야한다는 플래그를 표시하게 되며 시스템은 실행루프가 끝나고 다음 실행루프를 실행할때 뷰의 플래그를 보고 한번에 모든 제약조건을 갱신한다.
>따라서 호출하는 순간에 제약 조건이 바로 갱신되는것이 아니라 다음 실행루프에 한번만 갱신하게된다.
>한번의 실행루프에서 갱신이 여러번 일어난다면 비효울적이기 때문이다.
>
>setNeedsUpdateConstraints메서드를 호출하여 플래그를 표시하지않아도 시스템이 플래그를 표시하는경우도 존재한다!
>
>**updateConstraintsPermalink 호출해주는 트리거**
>
>1. View의 frame이 변경된 경우
>2. 뷰에 하위뷰가 추가되는 경우
>3. 디바이스의 orientation이 변경되는 경우(디바이스 회전한경우)
>4. Constraint Constant가 변경된 경우
>
>## updateConstraintsIfNeeded
>![](https://i.imgur.com/Dmxj3Q9.png)
>
>시스템은 제약의 갱신이 필요할 때 이 메서드를 호출하여 뷰와 하위뷰의 제약조건이 갱신되도록 한다.
>새로운 제약조건을 바로 적용해야할 경우 개발자가 수동으로 호출할수있다.
>해당 메서드는 다음 실행 루프가 아닌 호출된 순간에 제약조건을 갱신한다.
>
>## 2. Layout(레이아웃)
>## layoutSubviews
>![](https://i.imgur.com/USmIf3o.png)
>
>하위 뷰를 배치한다.
>
>하위 클래스는 하위 뷰의 더 정확한 레이아웃을 조정하기 위해 필요한 경우 이 메서드를 재정의할수 있다.
>하위뷰의 오토리사이징 및 오토레이아웃이 원하는 동작을 제공하지 않는 경우에만 이 방법을 재정의해야 한다.
>이 메서드에서 하위 뷰의 frame을 직접 설정할 수 있다.
>
>```
>레이아웃의 갱신은 뷰 계층구조에서 상위뷰에서 하위뷰의 순서로 이루어지기 때문에 상위뷰의
>layoutSubviews 메서드는 가장 먼저 호출해야한다.
>```
>이 함수는 직접 호출하면 안된다. updateConstraints 메서드와 같이 강제로 레이아웃을 갱신하고 싶을 경우 setNeedsLayouyt메서드를 호출하면 다음 실행루프에서 레이아웃이 갱신된다.
>즉시 레이아웃을 갱신하고 싶을때는 layoutIfNeeded 메서드를 호출할 수 있다.
>
>오토레이아웃을 사용할 때, 레이아웃 엔진은 제약 조건의 변화를 만족시키기 위해 필요에 따라 뷰의 위치를 갱신합니다.
>
>## requiresConstraintBasedLayoutPermalink
>![](https://i.imgur.com/6iGSo6X.png)
>
>뷰의 레이아웃이 제약 조건에 의존하는지 여부를 나타내는 Bool타입 연산 프로퍼티입니다.
> 뷰가 제약 조건 기반 레이아웃을 사용하고 제대로 작동할 경우 true를 반환합니다.
>
>```
>커스텀 뷰이고, 오토리사이징을 사용하여 올바른 레이아웃을 조정할 수 없는 경우 이 프로퍼티를
>true로 재정의해야 합니다.
>```
>
># 3. Draw
>## draw(_:)
>![](https://i.imgur.com/CKxOfzs.png)
>
>전달된 사각형 안에 수신자의 이미지를 그립니다..?
>전달된 사각형?은 파라미터로 받은 CGRect 타입을 의미한다.
>
>Core Graphics 및 UIKit 같은 기술을 사용하여 뷰의 내용을 그리는 하위 클래스는 이 메서드를 재정의하고 해당 드로잉 코드를 구현해야 합니다.
> 이 외의는 메서드를 재정의 할 필요가 없습니다.
> 예를들어, 뷰가 배경색만 표시하거나, 기본 레이어를 사용하여 직접 컨텐츠를 설정하는 경우에는 재정의 할 필요가 없습니다.
> 이 메서드는 뷰를 다시 그리는데만 사용되어야 하며, 레이아웃을 조정하거나 데이터를 조작하면 안됩니다.
>
>이 메서드는 뷰가 처음 표시될 때, 또는 뷰의 보이는 부분을 무효화하는 이벤트가 발생될 때 호출됩니다. 
>직접 이 메서드를 호출하면 안됩니다. 
>뷰의 일부분을 무효화하고 다시 그려지게 하려기 위해서는setNeedsDisplay 메서드를 호출할 수 있습니다.
>
>## Drawing and Updating the View
>func setNeedsDisplay(): 
>- 수신기의 전체 경계 사각형을 다시 그려야 하는 것으로 표시합니다.
>
>func setNeedsDisplay(CGRect)
>- 수신기의 지정된 사각형을 다시 그려야 하는 것으로 표시합니다.
>
>var contentScaleFactor: CGFloat
>- 뷰에 적용된 축척 비율입니다.
>
>func tintColorDidChange()
>- tintColor 속성이 변경 될 때 시스템에서 호출됩니다.
>
>## Related Documentation
>var contentMode: UIView.ContentMode
>- 경계가 변경될 때 뷰가 콘텐츠를 배치하는 방법을 결정하는 데 사용되는 플래그입니다.
>
>---
># UIViewController
>## 1. Constraints(제약조건)
>### updateViewConstraints
>뷰컨트롤러의 뷰가 제약 조건 갱신이 필요할 때 호출된다.
>UIView의 updateConstraints와 같다.
>
>## 2. Layout(레이아웃)
>### viewWillLayoutSubviews
>![](https://i.imgur.com/aHwus3H.png)
>
>뷰 컨트롤러의 뷰가 하위뷰의 레이아웃을 조정하려고 함을 알리기 위해 호출됩니다. 뷰의 bounds가 변경될 때, 하위 뷰의 위치를 조정합니다.
> 하위뷰의 레이아웃을 조정하기 전에 무언가 변경하기위해 뷰컨트롤러에 이 메서드를 재정의할 수 있습니다.
>  기본적으로 이 메서드에는 아무런 구현이 되어있지 않기 때문에 호출해도 어떤 일도 발생하지 않습니다.
>
>### viewDidLayoutSubviews
>![](https://i.imgur.com/3rr8CC8.png)
>
>뷰 컨트롤러의 뷰가 방금 하위뷰의 레이아웃을 조정했음을 알리기 위해 호출됩니다. 뷰의 bounds가 변경될 때, 하위뷰의 위치를 조정합니다.
>하지만 이 메서드는 하위뷰의 개별 레이아웃이 완벽하게 조정되었을 때 호출되지는 않습니다.
>따라서 각 하위뷰는 자신의 레이아웃을 조정할 책임이 있습니다.
>
>하위 뷰의 레이아웃이 모두 조정된 후에 무언가 변경하기위해 뷰컨트롤러에 이 메서드를 재정의할 수 있습니다.
> 기본적으로 이 메서드에는 아무런 구현이 되어있지 않기 때문에 호출해도 어떤 일도 발생하지 않습니다.
>
>---
># UIView의 레이아웃 사이클 메서드
>|사이클  |업데이트     |플래그|트리거  |
>|------|------------|---------|---------|
>|Constraints |updateConstraints|setNeedUpdateConstraints|updateConstraintsIfNeeded
>|Layout |layoutSubviews|setNeedsLayout|layoutIfNeeded
>|Draw  |draw|setNeedDisplay, setNeedDisplayInRect|없음
>
># Drawing Cycle
>>## UIView The View Drawing Cycle
>![](https://i.imgur.com/IEB0gA4.png)
>
>View drawing 은 필요할때 발생된다.
>
>뷰가 처음 보여질때 혹은 레이아웃 변경으로 인해 뷰의 전체 또는 일부가 화면에 보여지게되면 시스템에서 뷰의 내용을 그리도록 요청하게된다.
>(시스템:? 뷰 레이아웃 바꼇어 뷰 그려줘!)
>
>뷰의 값이 변경되면 뷰를 다시 그려야한다고 시스템에 알리는것은 개발자의 역할이다. 
>뷰의 setNeedsDisplay() 또는 setNeedsDisplay(_:) 메서드를 호출하여 이를 수행할수있다.
>
>이러한 메서드들은 시스템에게 다음 드로잉 사이클에 뷰를 업데이트 해야한다고 알린다.
>
>뷰를 업데이트하기 위해 다음 드로잉 주기까지 기다리기 때문에 여러 뷰에서 이러한 메서드를 호출하여 동시에 업데이트할 수 있다.
>
>setNeedsDisplay() 메서드는 시스템 한테 다음 드로잉 사이클때 View 업데이트 해줘!! 라고 하는것 같다.
>
>LayoutCycle의 순서대로 시스템이 호출해주는 메서드들을 확인해보자.
>
>## Drawing Cycle 이란? 
>**뷰가 로드 되거나 변경이 있을때 화면에 시각적으로 표현되어 그려지는 사이클이다.**
>
>1. View를 처음 표시하거나 View의 일부를 다시 그려야 하는경우 View의 draw(_:)메서드를 호출하여 View에 컨텐츠를 그려달라고 요청한다.
>
>2. View의 스냅샷을 캡쳐하여 UIView에 전달한다.
>
>3. View의 컨텐츠 변경시 관련 메서드(SetNeedsDisplay, setNeedsLayout 등등)를 호출하여 시스템에 업데이트하라고 요청한다.
>
>4. Next Drawing Cycle 에서 업데이트 요청 받은 뷰를 업데이트 한다.
>
>Drawing Cycle 이란 뷰의 스냅샷을 캡쳐하고 뿌려주는 프로세스를 반복하는 과정이다!
>
>**View 컨텐츠 변경 관련한 업데이트 트리거의 종류(얘내 안됨)**
>1. View를 부분적으로 가리고 있던 다른 View이동 또는 제거
>2. hidden 프로퍼티를 No로 설정하여, 이전에 숨겨진 View를 다시 볼 수 있게 만들기
>3. View를 화면밖으로 스크롤한다음, 화면으로 다시 이동하기
>4. View의 setNeedsDisplay 또는 setNeedsDisplayInRect: 메서드를 명시적으로 호출하기
>
>위의 상황들이 발생할경우 시스템이 현재 View의 스냅샷을 캡쳐 -> 컨텐츠가 변경되면 시스템에게 View를 업데이트하라고 요청 -> 현재 실행루프를 끝날때까지 대기하고 다음 드로잉 사이클에서 View 업데이트를 요청 받은 View 를 전부 업데이트 한다. 
>
>이때 View를 업데이트 할때 draw(_:) 메서드를 호출하여 View를 업데이트한다.
>
>위의 트리거들은 [developer.apple.com/archive](https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/GraphicsDrawingOverview/GraphicsDrawingOverview.html#//apple_ref/doc/uid/TP40010156-CH14-SW1) 에서 확인할수있는데 실험해보니...안된다.
>
>아카이브 마지막 업데이트 년도가 12년도다..
>
>![](https://i.imgur.com/AwGBv19.png)
>
>적용되는 트리거가 변경되었으나 업데이트가 안된것같다!
>이부분에대해 언급한 문서는 아직까지는 못찾았다..
>
>**View를 업데이트 하기위해서는 setNeedsDisplay, setNeedsDisplayInRect 등의 메서드를 호출해주어야 한다!**
>
>---
>## View Drawing Cycle 관련 메서드
>
>**View Drawing Cycle 관련 메서드**
>1. setNeedsDisplay() :
>- 뷰 내 요소들을 드로잉 해줌
>- 호출시점: 다음번 드로잉 사이클 시 (draw() 자동 호출)
>
>2. setNeedsLayout() :
>- 뷰 자체의 크기 및 위치를 레이아웃 시킴
>- 호출시점: 다음번 드로잉 사이클 시 (layoutSubviews() 자동 호출 )
>- layoutSubviews를 예약하는 행위 중 가장 비용이 적게 드는 방법이다
>
>3. displayIfNeeded() :
>- setNeedsDisplay와 같음 (layer의 내용을 강제로 업데이트)
>- 호출시점: Cycle을 기다리지 않고 즉시 실행
>
>4. layoutIfNeeded() :
>- setNeedsLayout()과 같은 기능 (호출 시점만 다르다)
>-호출시점: Cycle을 기다리지 않고 즉시 실행
>
>
>**setNeedsLayout과 layoutIfNeeded 의 차이점**
>
>setNeedsLayout과 layoutIfNeeded의 차이점은 동기적으로 동작하느냐 비동기적으로 동작하느냐의 차이 이다.


## 참고한 문서 및 자료
[UIView 공식문서](https://developer.apple.com/documentation/uikit/uiview)
[draw](https://developer.apple.com/documentation/uikit/uiview/1622529-draw)
[Drawing and Printing Guide for iOS](https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/GraphicsDrawingOverview/GraphicsDrawingOverview.html)
[View Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/CreatingViews/CreatingViews.html#//apple_ref/doc/uid/TP40009503-CH5-SW1)
[WWDC 2018 - High Performance Auto Layout](https://developer.apple.com/videos/play/wwdc2018/220/)
