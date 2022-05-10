# Today I Learned.

## 학습내용
# CollectionView DataSource, Delegate

## CollectionView DataSource
>컬렉션뷰 데이터 소스 객체는 컬렉션뷰와 관련하여 가장 중요한 객체다.
>필수로 구현해야한다.
>
>컬렉션 뷰의 콘텐츠(데이터)를 관리하고 해당 콘텐츠를 표현하는데 필요한 뷰를 만든다.
>
>데이터 소스 객체를 구현하려면 `UICollectionViewDataSource` 프로토콜을 준수하는 객체를 만들어야한다.
>
>그후 데이터 소스에는 필수로 구현해야하는 메서드가 2개 있다.
>
>### 필수 메서드
>**1. 지정된 섹션에 표시할 항목의 개수를 묻는 메서드**
>```swift
>func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int
>```
>**2. 컬렉션뷰의 지정된 위치에 표시할 셀을 요청하는 메서드**
>```swift
>func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
>```
>### 주요 선택 메서드
>**필수는 아니지만 상황에따라 구현해주어야 하는 메서드**
>
>**1. 컬렉션뷰의 섹션의 개수를 묻는 메서드**
>이놈 구현 안하면 섹션 개수는 1개가 기본값이다.
>```swift
> optional func numberOfSections(in collectionView: UICollectionView) -> Int
> ```
> **2. 지정된 위치의 항목을 컬렉션뷰의 다른 위치로 이동할 수 있는지 묻는 메서드**
> ```swift
>  optional func collectionView(_ collectionView: UICollectionView, canMoveItemAt indexPath: IndexPath) -> Bool
>  ```
> **3. 지정된 위치의 항목을 다른 위치로 이동하도록 지시하는 메서드**
> ```swift
> optional func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath)
> ``` 

## CollectionView Delegate
>컬렉션뷰 델리게이트 프로토콜은 컬렉션뷰에서 셀의 선택 및 강조표시를 관리하고 해당 셀에 대한 작업을 수행할수 있는 메서드를 정의한다.
>요놈은 선택사항이다.
>
>**주요 메서드**
>1. 지정된 셀이 사용자에 의해 선택될 수 있는지 묻는 메서드
>선택이 가능한경우 `true` 아닌경우 `false`
>```swift
>optional func collectionView(_ collectionView: UICollectionView, shouldSelectItemAt indexPath: IndexPath) -> Bool
>```
>2. 지정된 셀이 선택되었음을 알리는 메서드
>```swift
> optional func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath)
>```
>3. 지정된 셀의 선택이 해제될수 있는지 묻는 메서드
>선택 해제가 가능한경우 `true`, 아닌경우 `false`
>```swift
>optional func collectionView(_ collectionView: UICollectionView, shouldDeselectItemAt indexPath: IndexPath) -> Bool
>```
>4. 지정된 셀의 선택이 해제되었음을 알리는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, didDeselectItemAt indexPath: IndexPath)
>```
>5. 지정된 셀이 강조될수 있는지 묻는 메서드
>강조해야하는 경우 `true`아니면 `false`
>```swift
>optional func collectionView(_ collectionView: UICollectionView, shouldHighlightItemAt indexPath: IndexPath) -> Bool
>```
>6. 지정된 셀이 강조되었을떄 알려주는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, didHighlightItemAt indexPath: IndexPath)
>```
>7. 지정된 셀이 강조가 해제될때 알려주는 메서드
>```swift
>optional func collectionView(_ collectionView: UICollectionView, didUnhighlightItemAt indexPath: IndexPath)
>``` 
