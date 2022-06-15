# Today I Learned.

## 학습내용
# Diffable Datasource
>
>## DiffableDataSource란?
>
>WWDC19 에서 소개된 놈이다.
>
>IOS13 부터 사용가능하다!
>
>TableView 또는 CollectionView를 그리기 위한 데이터를 관리하고 UI를 업데이트 하는 역할을 한다.
> Data Source와 달리 데이터가 달라진 부분을 추적하여 자연스럽게 UI를 업데이트한다.
> 
>기본적으로, Diffable Data Source와 DataSource의 역할은 같다.
>
>하지만 Diffable Data Sources 를 사용하면 이전과 달라진 부분을 자동으로 알아차리고 새로운 부분만 다시 그리기 때문에 tableView나 collectionView를 단순하게 업데이트가 가능하다.
>
>**Diffable DataSource를 사용하여 얻게 되는 효과와 장점**
>- 추가적인 코드작업 없이도, 퀄리티 있는 에니메이션 적용이 가능하다.
>- 개선된 Data Source 매커니즘은 완벽하게 동기적인 버그나, 예외, 충돌 상황들을 피할 수 있게 해준다.
>- UI 데이터의 동기화 부분 대신 앱의 동적인 데이터와 내용에 집중할 수 있다.
>- identifier와 snapshot을 사용하는 간소화 된 Data 모델을 정의 하고, 이를 이용하여 UI를 업데이트 한다.
>
>**Diffable DataSource의 종류**
>- UITableViewDiffableDataSource
>- UICollectionViewDiffableDataSource
>
>**dataSource와 Diffable DataSource의 차이점**
>
>- dataSource: Protocol
>- Diffable DataSource: Generic Class
>
>하지만 Diffable DataSource 가 dataSource 를 conform 하고 있다.
>
>CollectionView, TableView 모두 동일하다
>
>## DiffableDataSource 를 사용하는 이유
>
>기존 TableView와 CollectionView의 dataSource 를 사용하면 프로토콜의 2가지 메서드를 필수로 구현해주어야 한다.
>
>- 각 section의 아이템의 개수 (the number of items)
>- cellForItemAt - cell의 render (content renders)
>
>![](https://i.imgur.com/HGMARg3.png)
>
>위의 코드 예시에서는 seciton의 개수 (the number of sections) 까지 구현해두었다.
>
>지금까지는 이런식으로 Controller 가 dataSource 를 Conform 해서 위와 같이 코드를 작성했었다.
>
>![](https://i.imgur.com/YWrmAA7.png)
>![](https://i.imgur.com/HcUpbyJ.png)
>
>collectionView 의 데이터를 구성하기위해 Controller 에게 numberOfSection 을 물어본다.
>
>controller 는 데이터를 채우기 위해 API 통신을 요청하고 응답을 받으면 collectionView 에게 변화를 알린다.
>
>이때 아래와 같은 에러를 마주하게 된다
>
>![](https://i.imgur.com/VKKeJEz.png)
>
>UI 와 Controller 의 데이터가 차이점이 생겨 발생하는 에러인듯하다..!
>
>이녀석을 해결하기 위해 reloadData 를 사용했었다.
>
>이처럼 reloadData 를 써서 Controller 와 UI 간의 동기화를 해주면 문제없이 사용할수 있지만 DiffableDataSource 를 사용한다면 reloadData 를 사용하는것보다 더 자연스러운 애니메이션(사용자경험)을 활용할수 있게된다.
>
>기존 reloadData 를 사용했을때는 TableView, CollectionView 를 한번에 업데이트 하므로 뚝뚝 끊어지는 UI 를 보게 된다.
>하지만 Diffable DataSource 를 사용하게되면 변경된 데이터 부분이 스르륵 사라지고 추가되는 UI 효과가 적용된다.
>
>위에 작성된 설명 자연스럽게 UI를 업데이트 한다 라고 설명된 이유다.
>
>## Diffable DataSource 를 사용해보자!
>![](https://i.imgur.com/qPwIp9I.png)
>
>Diffable DataSource 는 apply 라는 메서드를 사용한다.
>
>## Snapshot
>Diffable DataSource 를 사용하기위해 Snapshot 개념이 도입되었다.
>Snapshot 은 현재 상태? 라 할수있다.
>
>아래의 예제를 보자
>
>![](https://i.imgur.com/jGMnmdc.png)
>
>FOO, BAR, BIF가 있고, Controller가 변경되었다고 가정해보자.
>
>![](https://i.imgur.com/THPnotT.png)
>
>API 통신을해서 Controller 가 변경된다면 apply할 수 있는 새로운 snapshot이 생기게된다.
>
>apply 하게되면 새로운 Snapshot 이 적용된다.
>![](https://i.imgur.com/2v61DR6.png)
>
>이제 왜써야 하는지 개념은 알았으니 공식문서를 읽어보자!

# UITableViewDiffableDataSource
>![](https://i.imgur.com/9gdldAq.png)
>
>데이터를 관리하고 테이블 보기에 대한 셀을 제공하는 데 사용하는 개체입니다.
>
>## Declaration
>```swift
>@MainActor class >UITableViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType>:
> NSObject where SectionIdentifierType : Hashable, ItemIdentifierType : Hashable
>```
>
>## Overview
>diffable 데이터 소스 개체는 테이블 보기 개체와 함께 작동하는 특수한 유형의 데이터 소스입니다.
>간단하고 효율적인 방식으로 테이블 보기의 데이터 및 UI 업데이트를 관리하는 데 필요한 동작을 제공합니다.
>또한 UITableViewDataSource 프로토콜을 준수하고 프로토콜의 모든 메서드에 대한 구현을 제공합니다.
>
>테이블 보기를 데이터로 채우려면:
>1. diffable 데이터 소스를 테이블 보기에 연결하십시오.
>2. 테이블 보기의 셀을 구성하려면 셀 공급자를 구현하십시오.
>3. 데이터의 현재 상태를 생성합니다.
>4. UI에 데이터를 표시합니다.
>
>diffable 데이터 소스를 테이블 보기에 연결하려면 init(tableView:cellProvider:) 이니셜라이저를 사용하여 diffable 데이터 소스를 만들고 해당 데이터 소스와 연결하려는 테이블 보기를 전달합니다.
>
>또한 각 셀을 구성하여 UI에 데이터를 표시하는 방법을 결정하는 셀 공급자를 전달합니다.
>
>```swift
>dataSource = UITableViewDiffableDataSource<Int, UUID>(tableView: tableView) {
>    (tableView: UITableView, indexPath: IndexPath, itemIdentifier: UUID) -> UITableViewCell? in
>    // configure and return cell
>}
>```
>
>그런 다음 스냅샷을 구성하고 적용하여 데이터의 현재 상태를 생성하고 UI에 데이터를 표시합니다. 자세한 내용은 NSDiffableDataSourceSnapshot을 참조하세요.
>
>```
>important
>
>diffable 데이터 소스로 구성한 후에는 테이블 보기에서 dataSource를 변경하지 마십시오.
> 테이블 보기를 처음 구성한 후 새 데이터 원본이 필요한 경우 새 테이블 보기 및 diffable 데이터 원본을 만들고 구성합니다.
>```
>
>## TableViewDiffableDataSource 를 구현해보자!
>
>순서는 위에 작성된 공식문서의 순서대로 진행한다!
>
>1. diffable 데이터 소스를 테이블 보기에 연결하십시오.
>2. 테이블 보기의 셀을 구성하려면 셀 공급자를 구현하십시오.
>3. 데이터의 현재 상태를 생성합니다.
>4. UI에 데이터를 표시합니다.
>
>### DiffableDataSource 순서 1,2번 구현하기
>1. diffable 데이터 소스를 테이블 보기에 연결하십시오.
>2. 테이블 보기의 셀을 구성하려면 셀 공급자를 구현하십시오.
>처음으로 DiffableDataSource 를 만들어 tableView 의 dataSource 에 연결해주어야 한다!
>
>그렇다면 DiffableDataSource 을 만들어보자!
>위의 공식문서를 다시보자!
>```swift
>@MainActor class UITableViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType> :
> NSObject where SectionIdentifierType : Hashable, ItemIdentifierType : Hashable
>```
>
>DiffableDataSource 는 dataSource 와 다르게 제네릭 클래스다.
>위의 선언부를 보면 알수있듯이 제네릭 클래스 이기떄문에 적절한 타입을 넣어서 생성하면되는데
>
>이때 중요한것은 SectionIdentifierType과 ItemIdentifierType은 모두 Hashable 을 준수하는 타입만 들어갈수 있다.
>
>SectionIdentifierType 은 보통 Int 를 넣어주거나 Hashable 을 준수하는 타입을 만들어 넣어주면된다!
>섹션 어떻게 할건지 알려주는거라 생각하면됨!
>
>ItemIdentifierType은 어떤것을 보여줄것인지 라고 생각하면된다.
>
>아래의 코드는 사용예시
>
>```swift
>enum Section {
>     case main
>}
>
>struct DiaryData: Hashable {
>    let identifier = UUID()
>    
>    let title: String
>    let date: String
>    let body: String
>    
>    init(data: SampleData) {
>        title = data.title
>        date = Formatter.setUpDate(from: data.date)
>        body = data.body
>    }
>}
>
>private let tableView = UITableView()
>private var dataSource: UITableViewDiffableDataSource<Section, DiaryData>?
>
>private func setUpDataSource() {
>        dataSource = UITableViewDiffableDataSource<Section, DiaryData(tableView: tableView) {
>            tableView, indexPath, itemIdentifier in
>            
>            if let cell = tableView.dequeueReusableCell(withIdentifier: DiaryCell.identifier, for: indexPath) as? DiaryCell {
>                cell.configure(data: itemIdentifier)
>                return cell
>            }
>            
>            return tableView.dequeueReusableCell(withIdentifier: UITableViewCell.identifier, for: indexPath)
>        }
>    }
>```
>위의 예시를보고 추가설명을 하자면 dataSource 를 전역변수로두고 내부 로직을 함수로 빼두었다.
>
>제네릭 타입을 보면 <Section, DiaryData> 라고 되어있는데 테이블뷰에 보여질 섹션은 Section 타입으로 관리할거고 테이블뷰에서 보여줄 컨텐츠는 DiaryData 를 사용하겠다는 뜻이다.
>
>![](https://i.imgur.com/R4E95Xb.png)
>
>DiffableDataSource 의 내부로직을 보면 tableView 에는 어떤 tableView 랑 연결시킬건지 넣어주면되고! 나머지 뒷부분은 클로저로 작성하면된다!
>cellProvider는 tableView, indexPath, itemIdentifier 3개의 파라미터를 제공한다.
>
>저놈들 활용해주면된다!
>이부분은 dataSource 와 거의 흡사하니 넘어가도록하자!
>
>아 그리고 tableView에 Cell이 register 된 상태여야한다
>
>```swift
>tableView.register(DiaryCell.self, forCellReuseIdentifier: DiaryCell.identifier)
>```
>### DiffableDataSource 순서 3번 구현하기
>3. 데이터의 현재 상태를 생성합니다.
>
>이번 파트가 Snapshot을 사용하는 부분이다.
>NSDiffableDataSourceSnapshot 을 만들어주면되는데 이녀석은
>특정 시점의 데이터 상태를 나타내는 녀석이라고 생각하면된다.
>
>```swift
>struct NSDiffableDataSourceSnapshot<SectionIdentifierType, ItemIdentifierType>
> where SectionIdentifierType :Hashable, ItemIdentifierType : Hashable
>```
>snapshot은 Section과 item으로 구성된다. 
>아까 DiffableDataSource와 같이 제네릭 을 사용하는 구조체인데 제네릭 타입은 Diffable 처럼 Hashable을 준수하는 타입을 넣어주어야한다.
>
>완성된 예시코드를보자!
>```swift
>private func setUpSanpshot(data: [DiaryData]) {
>        var snapshot = NSDiffableDataSourceSnapshot<Section, DiaryData>()
>        
>        snapshot.appendSections([.main])
>        snapshot.appendItems(data)
>        
>        dataSource?.apply(snapshot)
>    }
>```
>제네릭 타입은 DiffableDataSource와 동일하게 만들어주었다.
>
>snapshot에 Section과 item을 추가한뒤 apply 해주면 변경된 값으로 보여지는 컨텐츠를 업데이트 시켜주는거다!
>
>section 에는 아까 만들어 두었던 Section타입의 main 을 넣어주었다.
>data 는 파라미터로 받을수 있도록 해주었다!
>
>메서드 호출할때 파라미터로 데이터만 넘겨주면 snapshot이 변경되도록 만들었다.
>
>### DiffableDataSource 순서 4번 구현하기
>4번은 큰거없다!
>위에서 만들어둔 setUpSanpshot() 메서드에 데이터 파라미터로 넣어서 호출해주면된다!
>
>```swift
>setUpSanpshot(data: diaryData)
>```
>![](https://i.imgur.com/6zlquYa.png)
>
>CollectionView의 DiffableDataSource사용법도 동일하다!

## 참고한 문서 및 자료
[WWDC2019 Advances in UI Data Sources](https://developer.apple.com/videos/play/wwdc2019/220/)
[UITableViewDiffableDataSourceTutorial](https://www.raywenderlich.com/8241072-ios-tutorial-collection-view-and-diffable-data-source)
[NSDiffableDataSourceSnapshot](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesnapshot)
[https://velog.io/@ellyheetov/UI-Diffable-Data-Source](https://velog.io/@ellyheetov/UI-Diffable-Data-Source)
[zeddios](https://zeddios.tistory.com/1197)
[UITableViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uitableviewdiffabledatasource)
