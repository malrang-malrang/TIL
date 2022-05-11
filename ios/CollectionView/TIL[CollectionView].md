# Today I Learned.

## 학습내용
# UICollectionView
> 컬렉션 뷰에 대해 알아보자!
## UICollectionView
> ![](https://i.imgur.com/7ZBlAVe.png)
>여러 데이터를 관리하고 커스텀 가능한 레이아웃을 사용해 사용자에게 보여줄수 있는 객체다.
>
>### Declaration
>```swift
>@MainActor class UICollectionView : UIScrollView
>```
>
>`UITableView` 처럼 `UIScrollView` 를 상속받고있다.
>
>컬렉션뷰는 테이블 뷰처럼 `UICollectionCell`을 사용하여 데이터를 화면에 표현한다.
>그외에도 `Supplementary view` 를 지원하여 아래의 예시사진 처럼 셀을 구분하여 표현할수있다.
>`Supplementary View` 에는 `Section`, `header`, `footer` 가있다.
>
>![](https://i.imgur.com/64wEZgE.png)
>
>**컬렉션뷰의 구성요소**
>![](https://i.imgur.com/CoGxuqK.png)
>
>### CollectionView 에서 각각의 역할들
>- `Cell`: 컬렉션 뷰의 콘텐츠를 표시한다. 각각의 셀은 컬렉션뷰 데이터 소스 객체에서 표시할 셀에 대한 정보를 가져온다. 각각의 셀은 `UICollectionViewCell` 클래스의 인스턴스 또는 `UICollectionViewCell` 을 상속받은 클래스의 인스턴스다.
>- `Supplymentary Views`: 섹션에 대한 정보를 표시한다. 구현이 필수는 아니다, 사용법과 배치 방식은 사용되는 레이아웃 객체가 제어한다. 헤더와 푸터를 예로 들수있다.
>- `Decoration views`: 콘텐츠가 스크롤되는 컬렉션뷰에 대한 배경을 꾸밀때 사용한다. 레이아웃 객체는 데코레이션 뷰를 사용하여 커스텀 배경 모양을 구현할수 있다.
>- `Layout Object`: 레이아웃 객체는 컬렉션뷰 내의 아이템 배치 및 시각적 스타일을 결정한다. 컬렉션뷰 데이터소스 객체가 뷰와 표시할 콘텐츠를 제공한다면, 레이아웃 객체는 크기, 위치 및 해당 뷰의 레이아웃과 관련된 특성들을 결정한다.
>
>### 컬렉션뷰 구현을 이루기 위한 클래스및 프로토콜
>컬렉션 뷰를 사용하여 콘텐츠를 화면에 표시하기 위해서 컬렉션뷰는 여러가지 다른 객체들과 협력한다.
>
>예를 들어 컬렉션뷰는 컬렉션뷰 데이터 소스 객체로 부터 표시할 콘텐츠의 정보를 얻어오고, 사용자와의 상호작용을 처리하기 위해 컬렉션뷰 델리게이트 객체를 사용한다.
>
>**최상위 포함 및 관리(Top level containment)**
>- `UICollectionView`: 컬렉션뷰의 컨텐츠가 보이는 영역을 정의한다.
>- `UICollectionViewController`: 컬렉션뷰를 관리하는 뷰컨트롤러다.
>
>**콘텐츠관리**
>- `UICollectionViewDataSource protocol`: 컬렉션 뷰에 표시할 콘텐츠 정보를 제공하고 관리한다.(필수)
>- `UICollectionViewDelegate protocol`: 사용자와 콜렉션뷰 간의 상호작용을 관리한다.
>
>**표시(Presentation)**
>- `UICollectionReusableView`: 콘텐츠들은 모두 `UICollectionReusableView` 의 인스턴스여야 한다.(뷰를 재사용하여 성능을 향상시킨다.)
>
>### Layout
>**컬렉션 뷰는 레이아웃이 존해하는데 컬렉션뷰의 셀들이 나열되는 방식을 결정한다.**
>레이아웃에는 두가지가 존재한다.
>1. UICollectionViewLayout
>2. UICollectionViewFlowLayout
>
>아래에서 살펴보자!
>
>#### 1. UICollectionViewLayout
>- `UICollectionViewLayout`: `UICollectionViewLayout` 의 서브 클래스는 레이아웃 객체라고하며 컬렉션 뷰 내부의 셀 및 재사용 가능한 뷰의 위치, 크기 및 시각적 속성을 정의한다.
>- `UICollectionViewLayoutAttribute`: `UICollectionViewLayoutAttribute` 의 레이아웃 프로세스 중에 컬렉션뷰에 셀과 재사용가능한 뷰를 표시하는 위치와 방법을 알려준다.
>- `UICollectionViewUpdateItem`: `UICollectionViewUpdateItem` 이 삽입, 삭제, 혹은 컬렉션뷰내에서 이동할때마다 레이아웃 객체는 UICollectionViewLayoutItem 클래스의 인스턴스를 받는다.
>
>#### 2. UICollectionVeiwFlowLayout
>- `UICollectionVeiwFlowLayout`: 그리드 혹은 다른 라인기반(lined-based)레이아웃을 구현하는데 사용된다.
>- `UICollectionVeiwDelegateFlowLayout`: 클래스를 그대로 사용하거나 동적으로 커스터마이징 할 수 있는 플로우 델리게이트 객체와 함께 사용할수있다.
>
>### 컬렉션뷰와 관련된 클래스 및 프로토콜
>- UICollectionView : 사용자에게 보여질 컬렉션 형태의 뷰입니다. 
>- UICollectionViewCell : UICollectionView 인스턴스에 제공되는 데이터를 화면에 표시하는 역할을 담당합니다.
>- UICollectionReusableView : 뷰 재사용 메커니즘을 지원합니다.
>- UICollectionViewFlowLayout : 컬렉션뷰를 위한 디폴트 클래스로, 그리드 스타일로 셀들을 배치하도록 설계되어있습니다. scrollDirection 프로퍼티를 통해 수평 및 수직 스크롤을 지원합니다.
>- UICollectionViewLayoutAttributes : 컬렉션뷰 내의 지정된 아이템의 레이아웃 관련 속성을 관리합니다.
>- UICollectionViewDataSource 프로토콜 : 컬렉션뷰에 필요한 데이터 및 뷰를 제공하기 위한 기능을 정의한 프로토콜입니다.
>- UICollectionViewDelegate 프로토콜 : 컬렉션뷰에서 아이템의 선택 및 강조 표시를 관리하고 해당 아이템에 대한 작업을 수행할 수 있는 기능을 정의한 프로토콜입니다.
>- UICollectionViewDelegateFlowLayout 프로토콜 : UICollectionViewLayout 객체와 함께 그리드 기반 레이아웃을 구현하기 위한 기능을 정의한 프로토콜입니다.

## 참고문서 및 자료
[UICollectionView](https://developer.apple.com/documentation/uikit/uicollectionview)
[부스트코스](https://www.boostcourse.org/mo326/lecture/16906)
