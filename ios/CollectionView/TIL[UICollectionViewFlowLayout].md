# Today I Learned.

## 학습내용
# UICollectionViewFlowLayout
>그리드뷰, 혹은 라인기반(line-based)레이아웃을 구현하는데 사용되는것.

## UICollectionViewFlowLayout
>`UICollectionViewFlowLayout`를 사용하면 컬렉션뷰의 셀을 원하는 형태로 정렬할수있다.
>
>플로우 레이아웃은 레이아웃 객체가 셀을 선형 경로에 배치하고 최대한 이 행을 따라 많은 셀을 채우는것을 의미한다.
>현재 행에서 레이아웃 객체의 공간이 부족하면 새로운 행을 생성하고 거기에서 레이아웃 프로세스를 게속 진행한다.
>
>### 플로우 레이아웃의 수직 스크롤
>![](https://i.imgur.com/p1W6Y8N.png)
>
>### 플로우 레이아웃의 수평 스크롤
>![](https://i.imgur.com/Rvb0vl2.png)
>
>플로우 레이아웃을 사용해 그리드 형태 말고도 다양한 레이아웃을 구현할수있다.
>
>### 플로우 레이아웃의 구성 단계
>1. 플로우 레이아웃 객체를 작성해 컬렉션뷰의 레이아웃 객체로 지정한다.
>2. 셀의 너비와 높이를 구성한다.
>3. 필요한 경우 셀의 간격을 조절한다.
>4. 원할 경우 섹션 헤더 혹은 섹션 푸터의 크기를 지정한다.
>5. 레이아웃의 스크롤 방향을 설정한다.
>
>### 플로우 레이아웃 속성 변경하기 
>콘텐츠의 모양을 구성하기위해 여러가지 프로퍼티를 제공한다.
>이 프로퍼티에 적절한 값을 설정하면 모든 셀에 동일한 레이아웃이 적용된다.
>예를 들면 플로우 레이아웃 객체의 itemSize 프로퍼티를 사용하여 셀의 크기를 설정할 경우 모든 셀의 크기가 동일하게 정용된다.
>
>### 플로우 레이아웃의 셀크기 지정하기
>컬렉션뷰의 모든셀이 같은 크기인 경우 플로우 레이아웃 객체의 itemSize 의 프로퍼티에 적절한 너비와 높이 값을 할당한다.
>각각의 셀마다 다른 크기를 지정하려면 컬렉션뷰 델리게이트 에서 `collectionView:layout:sizeForItemAtIndexPath:` 메서드를 구현해야한다.
>
>메서드의 매개변수로 제공하는 인덱스 결로 정보를 사용해 해당 셀의 크기를 반환할수있다.
>![](https://i.imgur.com/53LLAnG.png)
>셀마다 다른 크기를 지정하게되면 행에 있는 셀의 수는 행마다 달라질수있다.
>
>### 셀 및 행의 사이 간격 지정하기
>플로우 레이아웃을 사용하여 같은 행의 셀사이의 최소 간격과 연속하는 행 사이의 최소 간격을 지정할수 있다.
>행끼리의 간격은 플로우 레이아웃 객체에서 셀끼리의 간격에서와 같은 방법을 사용한다.
>모든셀의 크기가 같다면 플로우 레이아웃은 행 간격의 최솟값을 절대적으로 수용하며 하나의 행에 있는 모든 셀이 다음행의 셀과 균등한 간격을 유지할 수 있다.
>
>### 콘텐츠 여백 수정하기 
>섹션 인셋은 셀을 배치할때 여백공간을 조절하는 방법중에 하나다.
>인셋을 사용해 섹션 헤더뷰 다음과 푸터뷰 앞에 공간을 삽입할 수 있다.
>콘텐츠의 면 주위에 공간을 삽입할 수도 있다.
>인셋은 셀 배치에 있어 사용 가능한 공간을 줄이기 때문에 이를 사용하여 주어진 행의 셀 수를 제한할수도 있다.
>```swift
>let inset = UIEdgeInsetsMake(top, left, botom, right)
>```
>![](https://i.imgur.com/6ue79V0.png)
>
>### 셀의 예상(Estimated) 크기지정
>셀에 오토레이아웃을 적용하고 셀 스스로 크기를 결정한후 이를 UICollectionViewLayout 객체에 알려준다.
>이때 `estimatedItemSize` 프로퍼티를 사용해 대략적인 셀의 최소 크기를 미리 알려준다.
>```swift
>let flowLayout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
>flowLayout.estimatedItemSize = CGSize(width: 50.0, height: 50.0)
>collectionView.collectionViewLayout = flowLayout
>```
>
>### UICollectionViewDelegateFlowLayout
>요녀석은 `UICollectionViewFlowLayout`와 상호 작용하여 레이아웃을 조정할수 있는 메서드가 정의되어있다.
>이프로토콜의 메서드는 셀의 크기와 셀 간의 사이간격을 정의한다.
>모두 선택사항이다.
>
>**주요 선택 메서드** 
>1. 지정된 셀의 크기를 반환하는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, 
>                   layout collectionViewLayout: UICollectionViewLayout, 
>            sizeForItemAt indexPath: IndexPath) -> CGSize
>```
>2. 지정된 섹션의 여백을 반환하는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, 
>                   layout collectionViewLayout: UICollectionViewLayout, 
>                   insetForSectionAt section: Int) -> UIEdgeInsets
>```
>3. 지정된 섹션의 행 사이 간격 최소 간격을 반환하는 메서드
>**ScrollDirection 이 horizontal 이면 수직 행이되고 vertical 이면 수평이된다.
>```swift
>optional func collectionView(_ collectionView: UICollectionView, 
>                   layout collectionViewLayout: UICollectionViewLayout,
>                   minimumLineSpacingForSectionAt section: Int) -> CGFloat
>```
>4. 지정된 섹션의 셀 사이의 최소 간격을 반환하는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, 
>                   layout collectionViewLayout: UICollectionViewLayout,
>                   minimumInteritemSpacingForSectionAt section: Int) -> CGFloat
>```
>5. 지정된 섹션의 헤더뷰의 크기를 반환하는 메서드
>크기를 지정하지 않으면 화면에 보이지 않는다.
>```swift
>optional func collectionView(_ collectionView: UICollectionView, 
>                   layout collectionViewLayout: UICollectionViewLayout,
>                   referenceSizeForHeaderInSection section: Int) -> CGSize
>```
