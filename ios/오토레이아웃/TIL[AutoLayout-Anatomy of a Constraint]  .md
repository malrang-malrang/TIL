# Today I Learned

# **학습내용**
## AutoLayout
AutoLayout 의 핵심
Constraint(제약) 이 AutoLayout 의 핵심이다.
뷰 들에게 왼쪽에서 어느정도 떨어져있으면 좋겠어 라는 식으로 제약을걸어
앱을 실행할 기기의 화면 크기와 상관없이 유동적인 화면을 제공할수 있도록 한다.

## AutoLayout Attributes
오토레이아웃의 제약은 하나의 수식으로 나타낼수 있다.

![](https://i.imgur.com/e6PeS28.png)

수식을 보자면 RedView.Leading = 1.0 x BlueView.trailing + 8.0 
여기서 RedView 는 목적지이며 Leading 는 앞머리(왼쪽) 을 의미한다.
RedView 는 BlueView 의 Trailing(오른쪽) 에서 8.0 포인트 떨어진곳에 위치한다. 라고 해석할수있다.

Relationship 은 관계를 뜻함. (=) 말고 다른것도 있음.

## Attributes 의 종류
![](https://i.imgur.com/c1LXhgQ.png)

Attributes 의 종류 는 크게 2가지로 나타낼수 있음.
1. 뷰의 크기를 나타내는 Attributes
- Width(넓이)
- Height(크기)
2. 뷰의 위치를 나타내는 Attributes
- Top
- Bottom
- Center Y
- Center X
- Baseline(글자의 밑바닥) 을 의미함.
- Leading(글의 시작하는 방향) 을 의미함.
- Trailing(글이 끝나는 방향) 을 의미함.

중동국가 등 글을 오른쪽부터 왼쪽으로 작성하는 국가가 있기에 leading, Trailing 이라는 표현을 사용하게됨.

3. Not An Attributes

제약조건
- 크기 Attributes 와 위치 Attributes 를 섞어서 사용 불가능 하다.
- location Attributes 에는 constant(상수) value 를 할당할수없다.
- 수평 방향의 Attributes 는 수직 방향의 Attributes 에 Constraint(제약) 을 걸수없다. 반대도 마찬가지

### 사용예시
![](https://i.imgur.com/F8vLees.png)
![](https://i.imgur.com/4urSxEP.png)
말로 읽게되면 쉽다..!

1번 예시 
- view 의 높이는 40 이다.
2번 예시 
- 2번버튼의 leading 은 1번 버튼의 trailing 에서 8.0 만큼 떨어져 있다.
3번 예시
- 1번 버튼의 leading 은 2번 버튼의 leading 의 위치와 같다.(겹치지 않고 View 의 위쪽이나 아래쪽에 위치할수 있겠다.)
4번 예시
- 1번 버튼의 넓이 는 2번 버튼의 넓이와 같다.
5번예시 (superView 란 view 위에 버튼이 위치한경우 여기서 superView 는 view를 의미한다.)
-  view 의 Center X 는 SuperView 의 Center X 와 같다.
-  view 의 Center Y 는 SuperView 의 Center Y 와 같다.
- (앱의 화면에 superView 가 정중앙에 위치해있고 view 가 superView 위에 위치해있으며 앱의 화면에 정중앙에 위치해있다 는것을 의미한다.)
6번 예시(aspect ratio(가로세로 비율))
- view 의 높이는 view 의 가로 x2 이다.
- 1:2 의 비율을 의믜 한다. 높이가 2배!

### Equality, Not Assignment

위의 예시와 같은 동작 이지만 다른 표현 방식의 예시

![](https://i.imgur.com/T72Cwr7.png)

## Creating Nonambiguous, Satisfiable Layout (오토레이아웃 명확하게 지시하기)

오토레이아웃을 사용할때 목표는 조건을 뷰에게 걸어주어 크기나 위치를 찾아 가도록 하는것.
이때 조건이 하나라도 모호하거나 없게되면 자신의 크기나 위치를 알아낼수 없게 된다.

컴퓨터는 정확히 명령 대로 움직이기에 하나라도 부족하면 안된다.

### 오토레이아웃 설정시 생각해야할것
- view x, y 축의 시작점은 어떻게 알려줄수 있을까? 
- view x, y 축의 끝나는점은 어떻게 알려줄수 있을까? 
- view x, y 축의 넓이 는 어떻게 알려줄수 있을까?

### 예제
#### 세로 화면일때와 가로 화면일때 오토레이아웃 설정하기
![](https://i.imgur.com/HaBZsJF.png)

- 1view 가 다른 2view 와 관계를 형성하게되면 2view 는 1view 와 관계가 형성된 부분에는 조건을 걸어주지 않아도된다.
- 조건을 수정하기위해 같은 조건을 방식으로 설정하게되면 수정되는 것이아닌 추가 되는것이기 때문에 모호해져 에러가 발생됨.(기존에 있던 조건을 수정 하던지 삭제후 새로운 조건을 추가해주면 된다.)

2view 가 1view 와 같은 넓이 를 설정해줌으로써 2view 의 넓이를 설정 하지 않아도 동일한 넓이를 같게 된다.

2view 가 x축의 제약만 가지고 있다면 높이 설정이 안되어있기 때문에 에러를 발생한다. 이때 1view 와 top, bottom 을 정렬 해줌으로써 y 축의 설정값을 받기 때문에 높이가 1 view와 같아지게된다.

같은 화면이 보이더라도 오토레이아웃 설정 방법은 여러가지가 있을수 있다.

위의 예시를 코드로 풀어 작성하게되면 이렇게 된다.

![](https://i.imgur.com/xopYhti.png)

위와 같은 방식 말고 다른 방식도 가능하며 어떤 것이 좋다 라고 할순 없다.
각각 장단점을 가지고 있을뿐.

위의 예시대로 오토레이아웃을 설정 하게되면 1view 가 사라질경우 2view는 1view 의 top, bottom 을 정렬시켜둔 설정을 가지고 있기에 에러가 발생될수 있지만 반대로 1view 의 y 를 수정하게되면 2view 도 같은 y 값으로 수정되는 장점을 가질수있다.

## Constraint Inequalities (양쪽의 수식이 항상 동등하지 않을수 있다.)

관계가 항상 등호(=) 만 있는 것이아니다.

![](https://i.imgur.com/mQusi7m.png)

최소한의 사이즈를 정해주거나 최대한의 사이즈를 정해주고 싶을때 사용할수 있다.

1번 예시 
- view 의 넓이가 40보다 크거나 같았으면 좋겠다.
2번 예시
- view 의 넓이가 280보다 작거나 같았으면 좋겠다.
예시대로 조건을 걸어주게 되면 뷰의 크기는 40 이상 280 이하가 되게 된다.

이것만으로는 조건이 명확하지 않기때문에 크기를 특정지을수 없어 에러가 발생할수 있으나 다른 view 와 관계를 형성하여 조건을 주어 해결할수있다.

## Constraint Priorities (Constraint 의 우선도)

조건에 우선을 줄수 있다.
이게 왜 필요하지???

같은 조건을 걸었지만 값이 다른경우 우선도가 높은 쪽이 적용된다.
우선도가 낮은쪽이 항상 무시되는것은 아니며 적용되어야할 시점에 두개의 조건이 충돌 한다면 우선도가 높은쪽이 적용되는 것 이다.

우선도는 1 ~ 1000 의 값을 가질수 있으며1번 조건이 2번 조건보다 우선도가 1 만큼 더 높더라도 1번 조건이 우선도가 높은것으로 취급 된다.

## Intrinsic Content Size (고유 콘텐츠 사이즈)

콘텐츠의 사이즈를 가지고 뷰의 사이즈를 정해줄 거다 라고 이해하면 될것같다.

view 는 내부에 콘텐츠가 어떤것이 들어올지 모르기 때문에 Content Size 가 따로 없다.
button 같은 경우 버튼 내부에 Text 타이틀이 들어 갈수 있다.
이때 Text 타이틀이 Content 이기 때문에 Content Size 를 정해서 view 크기도 결정하도록 할수있다.
button 내부에 Content 의 크기 위치 를 조건을 걸어 줄수있다.


![](https://i.imgur.com/D1ATxBW.png)

위의 설명을 해석 하자면

- view 
    -  Content Size 를 정할수 없다.
- Label ,buttons, switches, and text fields
    -  text 를 가지고 있기 때문에 넓이와 높이를 조건으로 지정하지 않아도 자신의 사이즈를 추측을 할수있다.(위치만 잡아주면 사이즈는 알아서 정할거다)
- Text views and image views
    - Content Size 가 다양하게 측정이 될수있다.
- Slider
    - 높이만 정의가 되어 있다.

Content Size 를 기반으로 사이즈가 정해질거다. 라는 뜻으로 이해했다.

button text 의 경우 text 가 많아지거나 폰트가 커질경우 버튼의 크기가 알아서 더 커질수 있으며 image 같은 경우 이미지의 크기에 따라 이미지뷰의 크기가 달라질수 있겠다.

textview 의 경우 스크롤이 가능하냐 불가능하냐 에 따라서도 view 의 사이즈가 달라질수 있겠다.

![](https://i.imgur.com/woeO8P6.png)

- Label 의 경우 Content Size 에 따라 크기가 결정 되어 위치만 조건을 주어도 에러가 안생긴다.
- view 의 경우 Content Size 로 크기를 결정 할수 없기때문에 위치 에 대한 조건을 주었지만 크기가 결정되지 않았기 때문에 에러를 발생하게 된다.
- Slider 의 경우 크기중 높이는 Content Size 로 정의되어 위치만 잡아줘도 되지만 넓이는 정의 되어있지 않기 때문에 넓이도 조건을 주어 크기를 결정하도록 해야한다.

placeholder 를 이용해 view 의 크기를 임시로 고정해줄수있다.
하지만 앱이 실행될때는 적용되지 않으니 주의 하자.
스토리 보드 에서만 적용된다.

앱이 실행되었을떄도 크기를 적용 하는 방법
view 를 코드 파일과 연결하여 class : UIView 내부에서 Intrinsic Content Size 의 크기를 정해준다.
```swift 
import UIkit

@IBDesignable
class MyView: UIView {
    override var intrinsicContentSize: CGSize {
        return CGSize(width: 50, height: 50)
    }
}
```
이렇게 사이즈를 정해주게 되면 view 의 사이즈가 정의 되어있기 때문에 조건을 통해 위치만 알려주어도 에러가 발생되지 않는다.

## Content compression, Content hugging 
기준이 되는것은 Content size 이다.
우선도(Priorities) 를 기반으로 힘의 세기를 결정하게 된다.
각자 다른 개념으로 이해해야 한다.
늘어나면 보기 싫은 놈인지, 컨텐츠가 꼭 보여져야만 하는놈인지 고민해야한다.

### compression Resistance
줄어 들지 않으려고 버티는힘.
외부에서 view 를 찍어 누르려고 할때 content 가 버티는힘.
짤리지 않도록 버티는 힘. 가로 세로 설정이 가능하다.

### Content hugging 
늘어나지 않으려고 버티는 힘.
Content Size 에 꼭 맞게 줄어드는 힘.
(버튼을 예시로 들자면 text 의 크기만큼 버튼의 크기가 결정되도록 하는힘이 될수 있겠다. 타이트하게! text 보다 버튼의 크기가 더커서 남는 공간이 생기지 않도록 한다.)
 
#### 예시

각 label 을 높이와 leading 의 위치를 20 으로 설정해주었을 때 에러가난 상황이다.
마지막 label 만 trailing 의 값을 20 으로 조건을 하나 더 준 경우임.

![](https://i.imgur.com/acv2N23.png)

에러가 일어난 이유는 위치에 맞게 label 의 크기가 변경 되어야 하는데 어떤 label 이 늘어날지 알수없기 때문에 생겨난 에러이다.
이러한 상황에서 Content hugging 으로 에러를 해결할수 있다.
Content hugging 은 우선도(Priorities) 를 기반으로 결정되기 때문에 왼쪽 label 의 우선도를 높이게 되면 왼쪽 label 은 줄어들지 않으려는 힘이 다른 label 보다 강하기 때문에 크기를 유지하게되고 다른 label 의 크기가 달라지게된다.

![](https://i.imgur.com/FAKCrNh.png)
왼쪽 label 의 text가 늘어나서 크기가 더커질경우 에러가 발생된다.
이러한 상황에서 compression Resistance 으로 에러를 해결할수 있다.
왼쪽 label 은 우선도가 가장 낮고 중간 label 중간 우선도 오른쪽 label 은 높은 우선도를 설정 해준다면 에러를 해결할수 있다.
우선도가 가장 높은 오른쪽이 극단적으로 text의 양이 늘어나게되면 label 을 덮어버릴수도 있다.
