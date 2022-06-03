# Today I Learned.

## 학습내용
# UIRefreshControl
>
>![](https://i.imgur.com/Xou78YU.png)
>
>스크롤 보기의 내용 새로 고침을 시작할 수 있는 표준 컨트롤입니다.
>
>## Declaration
>```swift
>@MainActor class UIRefreshControl : UIControl
>```
>
>## OverView
>스크롤 뷰를 상속받는 애들이 사용할수 있으며 스크롤을 아래 방향으로 Pull(땡겨요!) 하게되면 로딩 애니메이션이 나오게 되고 로딩 애니메이션이 나왔을때 어떠한 행동을 하게 할것인지 정해줄수있으며 이를 활용해 새로고침 기능으로 사용할수 있게된다.
>
>![](https://i.imgur.com/cP8q1LI.png)
>
>
>## 공식문서 예제코드
>```swift
>func configureRefreshControl () {
>   // Add the refresh control to your UIScrollView object.
>   myScrollingView.refreshControl = UIRefreshControl()
>   myScrollingView.refreshControl?.addTarget(self, action:
>                                     #selector(handleRefreshControl),
>                                      for: .valueChanged)
>}
>    
>@objc func handleRefreshControl() {
>   // Update your content…
>
>   // Dismiss the refresh control.
>   DispatchQueue.main.async {
>      self.myScrollingView.refreshControl?.endRefreshing()
>   }
>}
>```
>사용 방법은 간단하다! `UIRefreshControl()`를 초기화한후 버튼을 추가하듯 타겟과 액션을 등록해주면 된다!
>
>## Relationships
>### 속성
>- var tintColor: UIColor!
>   - 새로고침 컨트롤러의 색상(애니메이션 색상 말하는거임)
>   
>- var attributedTitle: NSAttributedString?
>   - 새로 고침 컨트롤에 표시할 스타일이 지정된 제목 텍스트입니다.
>
>### 새로고침 상태 관리
>- func beginRefreshing()
>   - 새로 고침 작업이 프로그래밍 방식으로 시작되었음을 컨트롤에 알립니다.
>   
>- func endRefreshing()
>   - 새로 고침 작업이 종료되었음을 컨트롤에 알립니다.
>
>- var isRefreshing: Bool
>   - 새로 고침 작업이 트리거되어 진행 중인지 여부를 나타내는 부울 값입니다.

