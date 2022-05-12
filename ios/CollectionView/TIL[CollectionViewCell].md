# Today I Learned.

## 학습내용
# CollectionViewCell

## CollectionViewCell
>컬렉션뷰의 셀은 냉장고 속에 있는 반찬통으로 생각할수있다.
>
>컬렉션뷰라는 냉장고가있고, 냉장고 안에는 실제 반찬(콘텐츠)을 담고 있는 컬렉션뷰 셀이라는 반찬통이 있다고 생각할수 있다.
>
>- 컬렉션뷰 셀은 데이터 아이템을 화면에 표시한다.
>- 하나의 셀은 하나의 데이터 아이템을 화면에 표히한다.
>- 컬렉션뷰 셀은 두개의 배경을 표시하는 뷰와 하나의 콘텐츠를 표시하는 뷰로 구성되어있다.
>- 두개의 배경뷰는 셀이 선택되었을때 사용자에게 시각적인 표현을 제공하기 위해 사용된다.
>- 셀의 레이아웃은 컬렉션뷰의 레이아웃 객체에 의해 관리된다.
>- 컬렉션뷰 셀은 뷰의 재사용 메커니즘을 지원한다.
>- 일반적으로 컬렉션뷰 셀 클래스의 인스턴스는 직접 생성하지 않는다. 대신 특정 셀의 하위클래스를 컬렉션뷰 객체에 등록한 후, 컬렉션뷰 셀 클래스의 새로운 인스턴스가 필요할때, 컬렉션의 `dequeueReusableCell(withReuseIdentifier:for:)` 메서드를 호출한다.

## UICollectionViewCell 클래스
>**컬렉션뷰 셀의 구성요소 관련 프로퍼티**
>1. `var contentView: UIView`:
>- 셀의 콘텐츠를 표시하는 뷰.
>2. `var backgroundView: UIView?`:
>- 셀의 배경을 나타내는 뷰. 
>- 셀이 처음 로드되었을 경우 강조 표시되지 않거나 선택되지 않았을때 기본배경의 역할을 한다.
>3. `var selectedBackgroundView: UIView?`:
>- 셀이 선택되었을 때 배경뷰 위에 표시되는 뷰.
>- 해당 프로퍼니틑 셀이 강조 표시되거나 선택될때 기본 배경뷰 `backgorundView`를 대체하여 표시된다.
>
>**컬렉션 뷰 셀의 상태 관련 프로퍼티**
>1. `var isSelected: Bool`:
>- 셀이 선택되었는지를 나타낸다.
>- 셀이 선택되어있지 않다면 `false`
>2. `var isHighlighted: Bool`:
>- 셀의 하이라이트 상태를 나타낸다.
>- 하이라이트 되어있지 않다면 `false`
>
>**컬렉션뷰 셀의 드래그 상태 관련메서드**
>`func dragStateDidChange(_:)`:
>- 셀의 드래그 상태가 변경되면 호출된다.
>- 드래그의 상태는 `UICollectionViewCellDragState`의 열거형으로 표현되며 `none`, `lifting`, `dragging` 의 3가지 상태를 갖는다.
>
>## 컬렉션뷰 셀 과 테이블뷰 셀의 차이점
>1. 구조
> 테이블 뷰 셀의 구조는 콘텐츠 영역과 악세사리 뷰 영역으로 나뉘어있음.
> 컬렉션 뷰 셀의 구조는 배경뷰와 실제 콘텐츠를 나타내는 콘텐츠 뷰 로 나뉘어있음.
> 2. 레이아웃
> 테이블 뷰 셀은 목록형태로만 가능하다.
> 컬렉션 뷰 셀은 다양한 레이아웃을 지원한다.
>3. 스타일
>테이블 뷰 셀은 기본으로 제공되는 특정 스타일을 적용할수있다.
>컬렉션 뷰 셀은 제공되는 특정한 스타일이 없다.

## 참고 문서 및 자료
[야곰 부스트캠프](https://www.boostcourse.org/mo326/lecture/16907?isDesc=false)
[UICollectionViewCell
](https://developer.apple.com/documentation/uikit/uicollectionviewcell)
